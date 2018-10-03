+++
author = "Larissa"
categories = ["Python"]
tags = ["network-analysis", "twitter", "gephi", "spring2015", "german-only", "code"]
date = "2015-06-13"
description = "Ueber die Struktur von Hashtag-Netzwerken auf Twitter"
featured = ""
featuredpath = "date"
linktitle = ""
title = "Cluster oder Klumpen? -Python Code Appendix"
type = "post"
+++

```python
cd/Users/... #definiert den Zielordner, wo die Daten gespeichert und abgerufen werden sollen
import json #importiert das json-Paket, wird gebraucht um Daten dauerhaft abzuspeichern und wieder aufzurufen
from twython import Twython #importiert das Twython-Paket

def twitter_oauth_login(): #mit dieser Funktion wird man als Nutzer autorisiert, um auf die Inhalte von Twitter zuzugreifen
	API_KEY = "..."
	API_SECRET = "..."
	ACCESS_TOKEN = "..."
	ACCESS_TOKEN_SECRET = "..."
	twitter = Twython(API_KEY,API_SECRET,ACCESS_TOKEN,ACCESS_TOKEN_SECRET)
	return(twitter)
twitter = twitter_oauth_login()

def twitter_search(twitter_api, q, max_results=1000, **kw):
	search_results = twitter_api.search(q=q, count=100, result_type="mixed", **kw)
	statuses = search_results['statuses']
	max_results = min(1000, max_results)
		for _ in range(10):
			try:
				next_results = search_results['search_metadata']['next_results']
			except KeyError, e:
				break
			kwargs = dict([ kv.split('=') for kv in next_results[1:].split("&") ])
			search_results = twitter_api.search(**kwargs)
			statuses += search_results['statuses']
			if len(statuses) > max_results:
				break
	return statuses
	
def just_usernames(data): #diese Funktion wandelt die Unmenge an Informationen aus den Tweets in eine Liste um, in der nur die Screen Names der Autoren der Tweets stehen
	usernames = [p["user"]["screen_name"] for p in data]
	return usernames
	
def save_json(filename, data): #die Funktion dient dazu, in einer Variable gespeicherte Informationen als .json-Datei abzuspeichern
	with open(filename, "wb") as outfile:
		json.dump(data, outfile)

def get_minimum_id(list_of_tweets): #filtert aus einer Liste von Tweets die kleine Tweet-ID heraus
	ids = []
	for tweet in list_of_tweets:
		this_id = tweet['id']
		this_id = int(this_id)
		ids.append(this_id)
	minimum = min(ids)
	return minimum
	
def get_timeline(list_usernames): #zieht von jedem User aus einer Liste die komplette Timeline, im Moment allerdings eingestellt auf 20 Tweets pro User
	tweets_from_all_users = []
	i=1 #das Mitzählen der Aufrufe ist zwar eine sehr primitive, aber einfache Methode, um zu verhindern, dass das Limit überschritten wird
	for username in list_usernames:
		all_tweets = []
		max_id = None
		new_tweets = []17
		while True:
			try:
				new_tweets = twitter.get_user_timeline(screen_name=username, count=20, max_id=max_id) #will man die Anzahl der gezogenen Tweets erhöhen, muss man hier die 20 mit einer 100 ersetzen
			except Exception, exc:
				print "Error for fuer %s: %s" % (username, exc)
				break
			print u"Identified new tweets for user " + username
			i=i+1 #für jeden Abruf wird der Zähler eins nach oben gesetzt
			all_tweets.extend(new_tweets)
			min_id = get_minimum_id(new_tweets)
			max_id = min_id - 1
			if len(new_tweets) < 190: #die Zahl 190 hat sich als optimal herausgestellt, denn je kleiner die Zahl ist, desto häufiger erhält man die Fehlermeldung, dass die Liste leer sei, weil keine neuen Tweets mehr gefunden wurden. Sie darf allerdings auch nicht höher sein, da das Abrufen der Timeline manchmal 200 Ergebnisse pro Schub liefert, manchmal aber auch nur 198 oder 192 (siehe ...), obwohl noch weitere Tweets verfügbar wären
				print "Task complete: total tweets loaded."
				break
		tweets_from_all_users.append(all_tweets)
		if i == 178: #damit das Limit nicht überschritten wird
			print "wait"
			time.sleep(450)
			print "half" #um zu kontrollieren, dass das Skript noch läuft, es war bei meinem Rechner manchmal nicht auszuschließen, dass er sich einfach mittendrin aufhing
			time.sleep(450) #die Sleep-Zeit ergibt insgesamt die 15 Minuten, die man warten muss, bis das Limit zurückgesetzt wird
			i=1 #zurücksetzen, damit es bei der nächsten Schleife wieder funktioniert
	return tweets_from_all_users
	
def load_json(filename): #öffnet .json-Files beispielsweise in eine Variable18
	with open(filename) as f:
		data = f.read()
			try:
				data_json = json.loads(data)
			except Exception as e:
				print("Could not parse json data: {}".format(e.__str__()))
				data_json = {}
			finally:
				return(data_json)
				
def create_network_mentioning(data):
	network = []
	for entry in data:
		for tweet in entry:
			if 'entities' in tweet:
				if len(tweet['entities']['user_mentions']) > 0:
					mentioning_user = tweet['user']
					user_mentions = tweet['entities']['user_mentions']
					for mentioned_user in user_mentions:
						network.append([mentioning_user['id_str'], mentioned_user['id_str'],"1"])
	return(network)
	
def create_network_reply(data): #vorherige Funktion etwas umgeschrieben, damit es auch für die Replies klappt
	network = []
	for entry in data:
		for tweet in entry:
			if tweet['in_reply_to_user_id'] != None:
				mentioning_user = tweet['user']
				network.append([mentioning_user['id_str'], tweet['in_reply_to_user_id_str'],"1"]) #hier war die forSchleife von oben unnötig, da es nur einen Nutzer geben kann, auf den direkt geantwortet wird
	return(network)
	
def write_network(filename, network): #packt die Daten aus den zwei vorherigen Funktionen in eine übersichtliche Form, damit sie von Statistikprogrammen ausgelesen werden können
	delimiter = ','
	with open(filename, 'w') as out:
		for edge in network:
			line = delimiter.join(edge)
			out.write(line + "\n")

def usernames_into_network(data_in, data_out_mentioning, data_out_reply): #diese Funktion fasst alle Schritte zusammen, von der Liste der Nutzernamen bis zum Netzwerk ihrer Timeline
	usernames = load_json(data_in) #Nutzernamen-Liste wird aufgerufen
	usernames_set = set(usernames) #doppelte Nutzernamen werden gestrichen
	tweets = get_timeline(usernames_set) #für diese Nutzernamen wird die Timeline gerufen
	tweets_network_mentioning = create_network_mentioning(tweets) #findet bei jedem Tweet, ob in dem Text jemand erwähnt oder angesprochen wurde, und speichert die ID von beiden
	write_network(data_out_mentioning, tweets_network_mentioning) #packt die Erwähnungen in ein lesbares Netzwerk
	tweets_network_reply = create_network_reply(tweets) #findet bei jedem Tweet, ob es eine Antwort auf jemanden war, und speichert die ID von beiden
	write_network(data_out_reply, tweets_network_reply) #packt die IDs der Antwortenden und des Erwähnten in ein lesbares Netzwerk
	
# Durchführung (Beispiel):
# q = “#afdbpt”
# afd_results = twitter_search(twitter, q, max_results=1000)
# afd_usernames = usernames(afd_results)
# save_json(“afd_usernames.json”, afd_usernames)
# usernames_into_network(„afd_usernames.json“, „afd_network_mentioning.csv“, „afd_network_reply.csv“)
```