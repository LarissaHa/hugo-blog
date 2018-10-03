+++
author = "Larissa"
categories = ["Python", "Paper"]
tags = ["network-analysis", "twitter", "gephi", "spring2015", "german-only"]
date = "2015-06-13"
description = "Ueber die Struktur von Hashtag-Netzwerken auf Twitter"
featured = "abb1-abb2.png"
featuredpath = "date"
linktitle = ""
title = "Cluster oder Klumpen?"
type = "post"
+++

In dieser Arbeit soll es um die Vertiefung der Methoden gehen, die im Laufe des Seminars erworben wurden. Daf�r habe ich die Hashtags aus zwei Themengebieten untersucht und ihre Netzwerkstrukturen miteinander verglichen, um folgende Fragen zu beantworten: 

Unterscheiden sich die Netzwerke von Twitter-Usern, wenn sie �ber erschiedene Themen twittern? Finden sich verschiedene Netzwerk-Typen bei Politik und beispielsweise Sport, je nachdem um was es in der Konversation gerade geht?Und ist dies auch innerhalb eines Themas unterschiedlich, je nach benutztem Hashtag?

> Methoden der Politischen Soziologie II - Computational Social Science, Digital Methods and Big Data\\
> Fachbereich Politik\\
> Fakult�t f�r Sozialwissenschaften\\
> Universit�t Mannheim

# Mehr als nur Polarisierung?

Es kommt nicht oft vor, dass �ber amerikanischen Pr�sidentschaftswahlkampf berichtet wird,
ohne das Wort �Polarisierung� zu benutzen. Dieses Ph�nomen ist in der Politik allgegenw�rtig:
die Meinungen werden extremer, so sagt man. Unz�hlige Studien besch�ftigen sich deshalb mit
der Polarisierung von politisch interessierten Menschen, vor allem innerhalb ihrer sozialen
Netzwerke (siehe zum Beispiel Gentzkow/Shapiro 2011, Conover et al. 2014, Lawrence et al.
2010). Und laut der Theorie von Sol Stein wird sich diese Tendenz, im Laufe der Zeit und je mehr
das Internet Einzug in unsere Alltagswelt erh�lt, vermehren (Farrell 2012, 40).
Nun gibt es aber Forscher, die einen Schritt �ber die Schwarz-Wei�-Welt der Polarisierung
hinaus gehen wollen. Es gibt noch weitere Formen der politischen Interaktion, wie Smith et al.
zeigen (2014). Besonders gut lassen sich diese Formen �ber Twitter abbilden, denn obwohl die
Twitter-Nutzer kein repr�sentatives Abbild der Gesellschaft darstellen, so bildet dieser
Kurznachrichtendienst soziale Kontakte messbar auf technische Netzwerke ab und von dem
Verhalten der oft �berproportional politisch interessierten Nutzer k�nnte sich einiges auf die
ganze Gesellschaft �bertragen lassen. Ich werde die Analyse von Interaktionsstrukturen durch
Twitter-Nachrichten realisieren, auch deshalb weil es auf Twitter verschiedene Arten von
Interaktionsm�glichkeiten f�r die User gibt, die sich jeweils unabh�ngig messen lassen.
Die Frage, mit der ich mich deshalb im Folgenden besch�ftigen will, dreht sich um Interaktionen
auf Twitter und wer mit wem interagiert. Unterscheiden sich die Netzwerke von Twitter-Usern,
wenn sie �ber verschiedene Themen twittern? Finden sich verschiedene Netzwerk-Typen bei
Politik und beispielsweise Sport, je nachdem um was es in der Konversation gerade geht? Und
ist dies auch innerhalb eines Themas unterschiedlich, je nach benutztem Hashtag?
Um diese Fragen zu beantworten, habe ich eine Analyse von Twitter-Daten vorgenommen. Die
Daten wurden �ber die Twitter-API ausgelesen und mithilfe einer Netzwerkanalyse betrachtet.
So sollen R�ckschl�sse auf die Gesamtheit der Twitter-User und eventuell auch auf die Natur des
generellen politischen Diskussion m�glich werden.
Die untersuchten Hashtags wurden in die zwei Kategorien Politik und Sport eingeteilt, so geht es
einerseits um das Thema Griechenland und den Grexit, andererseits um die FIFAFrauenweltmeisterschaft in Kanada und die U21 Europameisterschaft in Tschechien.

# Grundlagen und bisherige Forschung

Die Forschung zur Polarisierung politische Diskurse reicht zur�ck bis in die 1940er Jahre. Bei
der Studie der Pr�sidentschaftswahlkampagne in den Vereinigten Staaten entdeckten die
Forscher, dass sich Rezipienten denjenigen Informationen eher zuwandten, die ihren vorher
geformten Einstellungen entsprachen. �ber Festinger, Sunstein und weitere bekannte Autoren
gewann das Thema immer weiter an Brisanz, bis sich das Internet verbreitete und der These
noch einen weiteren Ansto� gab (Stroud 2014, 1).
Das Internet gilt als Medium mit der h�chsten Auswahlm�glichkeit f�r den Rezipienten. In
keinem anderen Medium lassen sich die Informationen, die den Nutzer erreichen, so individuell
abstimmen. Sowohl im Fernsehen, im Radio als auch bei der Zeitung gibt es einen von der
Redaktion vordefinierten Informationsstrom, dem sich der Nutzer zwar zuwenden oder
abwenden kann, bei dem aber immer wieder Teile auftauchen, die er eigentlich vermieden h�tte.
In diesem Sinne ist das Internet eine Bereicherung und Erleichterung f�r den Nutzer, stellt aber
f�r die Kommunikations- und Demokratietheorie einen unklaren Faktor dar. Viele gingen bisher
davon aus, dass beispielsweise ein gemeinsam geschautes Fernsehprogramm ein Level f�r den
politischen Austausch darstelle (Schulz 2011, 131).
Mit dem Internet und der Tendenz der politischen Polarisierung wird dieses gemeinsame Level
gef�hrdet. Es gibt jedoch Autoren wie Smith et al., die davon ausgehen, dass es mehr als nur
Polarisierung im Internet gibt. Insgesamt finden die Autoren sechs verschiedene
Interaktionsstrukturen. Neben dem klassisch polarisierten Bild k�nnen sich die Nutzer auch
einig sein, fragmentieren, clustern oder in Hubs Informationen erhalten oder ihre Verbreitung
unterst�tzen (Smith et al. 2014).
Zun�chst gibt es die polarisierte Menge, ganz wie in der bisherigen Polarisierungsforschung
angenommen. Aber neben dieser Formation finden Smith et al. beispielsweise noch die �enge�
Menge, also eine Gemeinschaft, bei der sich keine klaren Untergruppen abgrenzen lassen.
Weiter geht es mit den �Brand Clusters�, die so hei�en, weil sich Twitter-Nutzer, die sich �ber
VIPs oder Produkte verschiedener Firmen austauschen, selten untereinander austauschen
sondern eher auf die ber�hmte Person oder die Firma verweisen. Im Vergleich dazu fanden
Smith et al. auch �Community Clusters�, die �hnliche Strukturen ausbilden, aber sich viel �fter
untereinander austauschen, sodass st�rkere Verbindungen zwischen den Mitgliedern der
Cluster entstehen. In den beiden letzten Netzwerkstrukturen gibt es jeweils einen zentralen
Akteur, der die Kommunikation mehr oder weniger bestimmt. Beim �Broadcast Network�5
sendet dieser zentrale Akteur Informationen an eine gro�e Menge verbundener Nutzer, die die
Information weiter verbreiten. Im Gegensatz dazu interagiert der zentrale Akteur im �Support
Network� mit den Nutzern, indem er auf ihre Nachrichten reagiert (Smith et al. 2014, 2-4).
Das Internet und Social Networking Dienste wie Facebook und Twitter eignen sich besonders
gut um Netzwerke zu analysieren. Musste man vorher umst�ndliche Umfragen durchf�hren, um
soziale Netzwerke zu erforschen, wurden diese nun mithilfe der neuen Technologie sichtbar
gemacht (f�r die Beschreibung verschiedener Netzwerk-Arten siehe Newman 2003). Soziale
Netzwerke werden zu messbaren Energieimpulsen. Dabei muss man auf Twitter aber zwischen
mehreren Interaktionstypen der Nutzer unterscheiden, denn nicht jede Interaktion hat dieselbe
Bedeutung, dem sollte man sich bei der Analyse und der Interpretation bewusst sein.
Zum einen gibt es die Follower/Following-Beziehungen. Ist ein Nutzer der Follower eines
anderen, so bedeutet das, dass er an den Nachrichten des anderen interessiert ist und �ber
dessen Aktivit�ten in seinem News-Feed informiert werden m�chte. Der �gefolgte� Nutzer hat
die M�glichkeit, seine Nachrichten direkt an seine Follower zu schicken und auf
schnellstm�glichem Wege zu verbreiten.
Zum anderen sind da noch die @messages oder auch @mentions. Mithilfe des @-Zeichens und
des Namens eines Nutzers l�sst sich direkt auf einen Nutzer verweisen oder diesen in einem
Tweet erw�hnen. Der erw�hnte Nutzer sieht den Tweet, genauso wie dessen und die eigenen
Follower. Steht das @-Zeichen jedoch am Anfang eines Tweets, so handelt es sich um eine
Antwort (Reply) und wird den eigenen Followern und die des angesprochenen nicht direkt
angezeigt. Man benutzt @messages meist, wenn man jemanden darauf aufmerksam machen will,
was man getweetet hat, oder wenn ein Kommentar zu jemandem oder einem anderen
Kommentar inhaltlich in Verbindung steht.
Au�erdem sind noch Retweets zu erw�hnen. Bei einem Retweet �bernimmt ein Nutzer den
Tweet eines anderen Nutzers mit der Notiz, von wem er den Tweet hat. Meist wird dadurch
Zustimmung ausgedr�ckt, da der Tweet fast wie ein eigener in die Tweet-�bersicht eingeht,
oder man m�chte seine Follower auf diesen Tweet hinweisen (f�r mehr Informationen, Metaxas
et al. 2014).
Zusammen mit den Hashtags bilden diese Funktionen das Kernger�st der Interaktion in
Twitter. Hashtags sind hierbei das Erw�hnen eines bestimmten Schlagwortes oder eines
Themas, signalisiert durch ein vorangestelltes #-Zeichen. Das nachgestellte Schlagwort wird6
automatisch zum Link, unter dem s�mtliche Tweets gesammelt werden, die ebenfalls das
Schlagwort mit #-Zeichen enthalten. So kann man auch auf andere Themen oder Nutzer
verweisen und seinen Tweet der ganzen Twitter-�ffentlichkeit (und nicht nur seinen eigenen
Followern) zug�nglich machen.
Viele Forscher haben sich bereits damit besch�ftigt und es hat sich gezeigt, dass es sich durchaus
lohnt, zwischen den verschiedenen Twitter-Funktionen in der Untersuchung zu unterscheiden
(Conover et al. 2012, Smith et al. 2014, Metaxas et al. 2014 etc.). So zeigen beispielsweise Smith
et al., dass in den einzelnen, in der Analyse gefundenen Nutzer-Cluster jeweils verschiedene
Hashtags und verschiedene Links benutzt wurden, um �ber das Thema zu sprechen.
Demokraten bezogen sich in der politischen Diskussion eher auf Mainstream-News-Webseiten,
w�hrend Republikaner vermehrt auf politische Kommentarseiten oder eher rechte NewsWebseiten verlinkten (Smith et al. 2014, 2). Conover et al. zeigten in ihrer Studie durchaus
polarisierte Kommunikationsstrukturen, die jedoch verschwanden, wenn man sich die FollowerBeziehungen ansah (Conover et al. 2012, 11). Dieses Ergebnis spricht daf�r, dass die Nutzer
zwar polarisiert waren, wenn es darum ging Inhalte zu verbreiten und zu kommentieren, die
Interessen aber breit verteilt waren und sich die Nutzer mehreren, nicht unbedingt homogenen
Informationsquellen zuwandten. Metaxas und Mustafaraj zeigten �hnliche Ergebnisse, allerdings
bei einer Untersuchung der @messages (Jungherr 2014, 53).
Ich werde mich bei meiner Analyse auf den Vergleich von @mentions und Replies
konzentrieren, da Follower/Following-Beziehungen zu starr sind und sich nur schlecht nach
Themen einteilen lassen und die Hashtags eher dazu dienen, die Themen der Netzwerke
einzugrenzen und zu bestimmen, als diese abzubilden. Wie eben erw�hnt, haben die Funktionen
unterschiedliche Bedeutung f�r den Nutzer. W�hrend man bei einem Reply in einem inhaltlichen
Kontext antwortet, der manchmal f�r den Au�enstehenden nicht so leicht erkennbar ist, so
lassen sich bei @messages eigene Gedanken und Kommentare anbringen und beliebige
Personen ansprechen, ohne dass bereits vorher eine Konversation zu dem Thema stattgefunden
haben muss.
Dieser kurze �berblick zeigt schon, dass die Ergebnisse durchaus variieren, je nach dem welches
Konzept man untersucht und auch in welchem Land man die Daten sammelt. Allein durch die in
jedem Land verschiedenen Wahl- und Mediensysteme k�nnen sich die Netzwerke
unterscheiden. Nat�rlich ist es mit vielen Hashtags klar, aus welchem Land der Gro�teil der
Tweets kommt (wenn man zum Beispiel Hashtags zur Pr�sidentschaftswahl nimmt), doch viele
Themen werden international diskutiert, sodass sich auch schlecht eine klare7
Parteizugeh�rigkeit zuordnen l�sst. So betrifft zum Beispiel das Thema �Grexit� vor allem die
Griechen, die auch aktiv dar�ber twittern, doch auch Nutzer aus den anderen europ�ischen
Staaten greifen das Thema auf und benutzen diesen zentralen Begriff. Es macht bei solchen
Themen wenig Sinn, die Analyse auf nur ein Land zu beschr�nken. Sehr wohl macht es aber Sinn,
zwischen verschiedenen Arten von Hashtags zu unterscheiden. Smith et al. haben bereits
gezeigt, dass unterschiedliche Themen angesprochen werden, je nachdem welche Gruppe von
Twitter-Nutzern untersucht wurde. Genauso werden auch die Nutzerstrukturen verschiedener
Hashtags nicht gleich aussehen.

# Netzwerkstrukturen im Vergleich

Ausgehend von dem eben erw�hnten theoretischen Hintergrund, erwarte ich verschiedene
Ergebnisse, die ich in den Hypothesen zusammengefasst habe. Da es im Rahmen dieser Arbeit zu
aufwendig w�re, um speziell f�r die einzelnen Interaktionsstrukturen zu testen, die Smith et al.
vorschlagen, werde ich meine Erwartungen auf allgemeinere Gesichtspunkte beziehen
H1: Die Netzwerke von nicht-politischen Hashtags sind enger und dichter als die von
politischen Hashtags.
Ich denke, dass es bei politischen Hashtags eher zu Cluster- oder Gruppen-Bildung kommt, da
die diskutierten Themen ein gr��eres Potential bieten, sich differenziert dar�ber
auszutauschen. Es k�nnte thematische oder ideologische Cluster geben, beispielsweise je nach
Parteizugeh�rigkeit. Je nachdem wie dieses Cluster aussieht, k�nnte man auch wieder eine
polarisierte Diskussion vermuten.
H2: Reply-Netzwerke sind fragmentierter und weniger dicht als @mention-Netzwerke.
Wie schon erw�hnt, bilden Replies eine Untergruppe der @mentions und finden meist in einem
bestimmten Kontext statt, n�mlich in dem Wissen um die Nachricht, auf die von einem Nutzer
geantwortet wurde. Demnach sind diese Interaktionen meist in sich geschlossen und weniger
untereinander verkn�pft.

# Ursprung des Datensatzes

Meine Analyse m�chte ich im Folgenden auf die Hashtags #grexit, #greferendum, #varoufakis
und #greece sowie #U21euro, #fifawwc, #GERFRA und #PORGER beschr�nken, die ebenfalls
international diskutiert wurden.8
#grexit bezieht sich auf den m�glichen Austritt Griechenlands aus der Eurozone, die
Entscheidung dieser Situation stand zum Zeitpunkt der Analyse kurz bevor. Die meisten Tweets
mit dem Hashtag #greece bezogen sich ebenfalls auf dieses Thema. #greferendum wurde das
geplante Referendum in Griechenland genannt, bei dem die Griechen selbst entscheiden sollten,
ob sie den Forderungen der anderen Euro-L�nder nachkommen wollten. Das Thema wurde in
den Medien sehr kontrovers diskutiert und war auch in Twitter ein popul�res Thema. Yanis
#varoufakis ist der Name des ehemaligen Finanzministers der Griechen. Er war die Person, die
sich am meisten und am �ffentlichsten gegen die Reformvorhaben aussprach und �ber den auch
oft getwittert wurde.
Zur gleichen Zeit fanden auch zwei sportliche Gro�ereignisse statt, zum einen die FIFA
Frauenweltmeisterschaft in Canada, zum einen die U21 Europameisterschaft in Tschechien.
Dazu werde ich die passenden Hashtags untersuchen, einmal #fifawwc (FIFA Women World
Championship) und #U21euro. Bei diesen sportlichen Ereignissen war jeweils ein Spiel aktuell,
dass sowohl deutschlandweit als auch international kommentiert wurde. Die deutsche
Frauennationalmannschaft spielte erfolgreich gegen Frankreich (#GERFRA), die U21-Herren
verloren katastrophal gegen die Portugal-U21 (#PORGER).
Diese Hashtags waren am Samstag, dem 27. Juni 2015, alle aktuell und teilweise in den TwitterTrends. Im Anschluss wurden die Nutzer untersucht, die diese Hashtags verwendeten, und wie
sie in ihrem Netzwerk positioniert sind, also wem sie @messages schrieben oder auf welche
Tweets sie antworteten. Von diesen acht Hashtags wurden 200 Tweets �ber die Twitter-API
abgerufen, die neue und popul�re Tweets gemischt enthielten. Um nun herauszufinden, in
welchen Netzwerkstrukturen diese Hashtags benutzt wurden, dienten die Autoren der 200
Tweets als Ausgangslage, um �ber deren Timeline die Interaktionsstrukturen zu untersuchen.
Um die praktische Durchf�hrbarkeit der Analyse zu gew�hrleisten, wurde die Anzahl der zu
untersuchender Tweets auf 20 pro Nutzer beschr�nkt. Bei einer umfassenderen Analyse kann
und sollte dieser Wert hochgesetzt werden.
Die nun gesammelten Tweets wurden auf Interaktionsmerkmale untersucht, also jeweils auf
@mentions und Replies, wobei klar separiert wurde, um die Ergebnisse sp�ter vergleichen zu
k�nnen. Dabei stellten die Replies logischerweise eine Untergruppe der @mentions dar, worauf
ich aber sp�ter noch einmal zur�ck kommen m�chte.
Mithilfe des Programms NodeXL war es schlie�lich m�glich, die stattgefundenen Interaktionen
in einer Netzwerkstruktur darzustellen und zu analysieren. Zus�tzlich wurde �ber Wakita-9
Tsurumi-Algorithmus die Anzahl und Auspr�gung m�glicher Gruppen berechnet, die in den
Grafiken farbig dargestellt werden.
Mein Skript basiert dabei vor allen Dingen auf den Skripten aus dem Kurs und eigenen
Bearbeitungen, wo sie ben�tigt wurden.

# Auswertungder Daten

## H1: Die Netzwerke von nicht-politischen Hashtags sind enger und dichter als die von
politischen Hashtags.

Wenn man sich die Netzwerke der einzelnen Hashtags betrachtet, sieht man gleich Unterschiede
zwischen den Themengebieten Politik und Sport. Abbildung 1 zeigt die Verteilung beim Hashtag
#greferendum, w�hrend Abbildung 2 #fifawwc zeigt. Man sieht deutlich, dass der Hashtag aus
dem Themenbereich Politik ein dichteres und engeres Netzwerk abzubilden scheint als der
Hashtag aus dem Themengebiet Sport. 

![png](/img/2015/06/abb1-abb2.png)

Dieser Eindruck setzt sich auch fir die Netzwerke der anderen Hashtags fort, je nachdem
welchem Themengebiet sie zuzuordnen sind. Bei sportlichen Hashtags kommt es auch �fter vor,
dass manche Untergruppen
komplett losgel�st vom
restlichen Netzwerk dargestellt
werden (siehe Abbildung 3,
#PORGER).
Diese Ergebnisse sind auch in
den berechneten Metriken
nachzuvollziehen. Die Dichte
(Density)[1], also der Koeffizient,
der berechnet wie stark die
einzelnen Netzpunkte
miteinander verkn�pft sind (Newman 2003, 12), ist bei den Sport-Hashtags durchgehend h�her
als bei den Politik-Hashtags (siehe Tabelle 1). Das bedeutet, dass diese Netzwerke st�rker in sich
selbst vernetzt sind. Dagegen ist auch der Wert des Geodesic path (Diameter) h�her. Dieser
Diameter gibt an, wie viele Schritte notwendig sind, um von einem Punkt des Netzwerkes zum
anderen zu gelangen (Newman 2003, 5). Das bedeutet, dass das Sport-Netzwerk zwar mehr
Verbindungen hat, aber insgesamt weniger in sich verbunden ist, da in den Politik-Netzwerken
die k�rzesten Wege im Vergleich k�rzer sind.
Ein weiterer auff�lliger Wert ist der der �Connected Components� (siehe Tabelle 1). Der Wert
gibt an, wie viele Punkte untereinander verbunden sind, jedoch nicht mit dem Rest des Graphen.
Vergleicht man nun die Auspr�gungen f�r Sport und Politik sieht man, dass die Politik-Hashtags
zwar durchweg mehr Vertices aufweisen, aber der Wert der Connected Components niedriger
ausf�llt, sie also weniger losgel�ste Teil-Netzwerke haben.

Auch die Art der Teilgruppen variiert deutlich zwischen den einzelnen Netzwerken, auch wenn
hier keine Regelm��igkeit beim Vergleich von Sport- und Politik-Hashtags festgestellt werden
konnte. Am Beispiel des Netzwerks f�r #fifawwc sieht man deutlich, dass es einerseits mehr
oder weniger zentralisierte Untergruppen gibt (siehe Abbildung 4), andererseits aber auch weit
verzweigte (siehe Abbildung 5).

## H2: Reply-Netzwerke sind fragmentierter und weniger dicht als @mentionNetzwerke.

Im Vergleich der zwei untersuchten
Interaktionsstrukturen sieht man auch gleich
Unterschiede. Die Netzwerke der Replies sind
weniger dicht und generell kleiner, da sie ja
auch ein Teilnetzwerk der @mentionNetzwerke sind. Typisch f�r die ReplyNetzwerke sind einzelne sternf�rmige
Interaktionen, bei denen zum Beispiel ein
Nutzer auf mehrere andere Tweets antwortete,
oder mehrere Nutzer auf einen besonderen
Tweet reagierten (siehe Abbildung 7).

# Schlussbetrachtung

Alles in allem zeigt die vorliegende Analyse, dass es sehr wohl Unterschiede zwischen den
Netzwerken verschiedener Hashtags und Themen gibt. Mit einer erweiterten Analyse k�nnte
man noch mehr �ber die Eigenschaften dieser Netzwerke sagen und wie sie sich im Laufe der
Zeit ver�ndern. Dann k�nnte man eventuell f�r jedes Thema oder f�r jeden Themenbereich eine
exakte Zuordnung zu den sechs Typen von Smith et al. vornehmen und vielleicht auch
Vorhersagen wagen, wie sich ein Hashtag in Zukunft entwickeln wird. Mit der geeigneten
Hardware lie�en sich die Zahlen der gesammelten Tweets vervielfachen, und nach dem Gesetz
der gro�en Zahlen, werden solche Modelle und Vorhersagen immer genauer, je mehr Daten in
die Analyse aufgenommen werden k�nnen. Zum Beispiel k�nnte untersucht werden, ob es
zentrale Akteure in den Netzwerken gibt, immer wiederkehrende Informationskan�le, oder
konversationsbestimmende "Influencers", wie Smith et al. sie nennt (Smith et al. 2014, 7).
In einem zweiten Schritt w�re es sicherlich auch interessant, der aktuellen Forschung zu folgen
und zu untersuchen, ob und wie sich die gezeigten Cluster auch inhaltlich voneinander
unterscheiden. Dies w�re beispielsweise durch eine Funktion von NodeXL machbar, mit der
automatisch Tweets gezogen und in verschiedenen Kategorien gemessen werden, unter
anderem Top Domains, Top Hashtags, Top Words und Top Mentions.
Es w�re beispielsweise interessant zu sehen, ob sich die Twitter-Nutzer zwar in Clustern
bewegen, aber innerhalb dieser Gruppen trotzdem �ber verschiedene Themen kontrovers
sprechen. Somit w�rde zwar die Struktur des Netzwerkes f�r eine fortschreitende
Fragmentierung des Diskurses sprechen, aber der Inhalt des Diskurses k�nnte aufkommende
Sorgen wie von Sol Stein zerstreuen.
Eine weitere M�glichkeit diese Analyse auszuweiten w�re, die Messung an mehreren
Zeitpunkten fortzusetzen, um so zu erfahren, ob sich die Interaktionsstruktur zu einem Hashtag
�ber die Zeit ver�ndert, oder ob sie gleich bleibt. Hashtags durchlaufen immer eine Karriere, von
dem Punkt an dem sie popul�r werden bis sie wieder an Aktualit�t verlieren. Diese Karriere
schl�gt sich mit Sicherheit auch in den Netzwerkstrukturen sichtbar nieder.
Auf jeden Fall bleibt bei allen Forschungsmethoden immer zu beachten, dass man einen
Kurznachrichtendienst untersucht. Reale Freundschaftsverh�ltnisse und Interessensneigungen
werden vielleicht teilweise abgebildet, aber eben mit Sicherheit nicht komplett. Man sollte nicht
den Fehler machen und die Ergebnisse eins zu eins auf die reale Welt �bertragen. Dennoch l�sst14
sich eventuell aus diesen Ergebnissen einiges schlie�en, vor allem wenn man bedenkt, dass es
immer mehr Leute gibt, die sich nur noch �ber das Internet �ber aktuelle Geschehnisse
informieren (f�r eine Diskussion der �bertragbarkeit siehe Metaxas/Mustafaraj 2012).
In Zukunft bleibt nur zu w�nschen, dass die aktuelle Forschung etwas zur Einheitlichkeit und
Vergleichbarkeit der Studien beitr�gt, damit eine nicht ganz so gro�e Unsicherheit dar�ber
besteht, welche Methode und welche Operationalisierung am sinnvollsten sind, und was dies im
Vergleich mit anderen Studien bedeutet.

# Appendix

## [Python Code](/blog/cluster-python-code)

# Fu�noten

[1] Die Dichte (Density) wird bei NodeXL durch den Vergleich der tats�chlichen Anzahl der Kanten mit der
h�chstm�glichen Anzahl der Kanten, wenn alle Eckpunkte miteinander verbunden w�ren, berechnet.

# Literatur

Conover, Michael D./Bruno Goncalves/Alessandro Flammini/Filippo Menczer. 2012. �Partisan
asymmetries in online political activity.� EPJ Data Science 1: 1-19.
Farrell, Henry. 2012. �The Consequences of the Internet for Politics.� Annual Review of Political
Science 15: 35-52.
Gentzkow, Matthew und Jesse M. Shapiro. 2011. �Ideological Segregation Online and Offline.� The
Quarterly Journal of Economics 126, 1799-1839.
Jungherr, Andreas. 2014. �Twitter in Politics: A Comprehensive Literature Review.� Social
Science Electronic Publishing. http://ssrn.com/abstract=2402443, letzter Zugriff am 11.07.2015
Lawrence, Eric, John Sides und Henry Farrell. 2010. �Self-Segregation or Deliberation? Blog
Readership, Participation, and Polarization in American Politics.� Perspectives on Politics 8 (1):
141-157.
Metaxas, Panagiotis T./Eni Mustafaraj. 2012. �Social Media and the Elections.� Science 338: 472-
473.
Metaxas, P. T./E. mustafaraj/K. Wong/L. Zeng/M. O�Keete/S. Finn. 2014. �Do Retweets indicate
Interest, Trust, Agreement? (Extended Abstract).� http://arxiv.org/abs/1411.3555, letzter
Zugriff am 11.07.2015.
Newman, M. E. J. 2003. �The structure and function of complex networks.� SIAM Review 45 (2):
167-256.
Schulz, Winfried. 2011. Politische Kommunikation. Theoretische Ans�tze und Ergebnisse
empirischer Forschung. 3. Auflage. Wiesbaden: Springer VS.
Smith, Marc A./Lee Rainie/Itai Himelboim/Ben Shneiderman. 2014. �Mapping Twitter Topic
Networks. From Polarized Crowds to Community Clusters.� PewResearchCenter.
http://www.pewinternet.org/2014/02/20/mapping-twitter-topic-networks-from-polarizedcrowds-to-community-clusters/, letzter Zugriff am 11.07.2015.
Stroud, Natalie Jomini. 2014. Selective Exposure Theories. The Oxford Handbook of Political
Communication (Forthcoming). Oxford: Oxford University Press.