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

In dieser Arbeit soll es um die Vertiefung der Methoden gehen, die im Laufe des Seminars erworben wurden. Dafür habe ich die Hashtags aus zwei Themengebieten untersucht und ihre Netzwerkstrukturen miteinander verglichen, um folgende Fragen zu beantworten: 

Unterscheiden sich die Netzwerke von Twitter-Usern, wenn sie über erschiedene Themen twittern? Finden sich verschiedene Netzwerk-Typen bei Politik und beispielsweise Sport, je nachdem um was es in der Konversation gerade geht?Und ist dies auch innerhalb eines Themas unterschiedlich, je nach benutztem Hashtag?

> Methoden der Politischen Soziologie II - Computational Social Science, Digital Methods and Big Data\\
> Fachbereich Politik\\
> Fakultät für Sozialwissenschaften\\
> Universität Mannheim

# Mehr als nur Polarisierung?

Es kommt nicht oft vor, dass über amerikanischen Präsidentschaftswahlkampf berichtet wird,
ohne das Wort „Polarisierung“ zu benutzen. Dieses Phänomen ist in der Politik allgegenwärtig:
die Meinungen werden extremer, so sagt man. Unzählige Studien beschäftigen sich deshalb mit
der Polarisierung von politisch interessierten Menschen, vor allem innerhalb ihrer sozialen
Netzwerke (siehe zum Beispiel Gentzkow/Shapiro 2011, Conover et al. 2014, Lawrence et al.
2010). Und laut der Theorie von Sol Stein wird sich diese Tendenz, im Laufe der Zeit und je mehr
das Internet Einzug in unsere Alltagswelt erhält, vermehren (Farrell 2012, 40).
Nun gibt es aber Forscher, die einen Schritt über die Schwarz-Weiß-Welt der Polarisierung
hinaus gehen wollen. Es gibt noch weitere Formen der politischen Interaktion, wie Smith et al.
zeigen (2014). Besonders gut lassen sich diese Formen über Twitter abbilden, denn obwohl die
Twitter-Nutzer kein repräsentatives Abbild der Gesellschaft darstellen, so bildet dieser
Kurznachrichtendienst soziale Kontakte messbar auf technische Netzwerke ab und von dem
Verhalten der oft überproportional politisch interessierten Nutzer könnte sich einiges auf die
ganze Gesellschaft übertragen lassen. Ich werde die Analyse von Interaktionsstrukturen durch
Twitter-Nachrichten realisieren, auch deshalb weil es auf Twitter verschiedene Arten von
Interaktionsmöglichkeiten für die User gibt, die sich jeweils unabhängig messen lassen.
Die Frage, mit der ich mich deshalb im Folgenden beschäftigen will, dreht sich um Interaktionen
auf Twitter und wer mit wem interagiert. Unterscheiden sich die Netzwerke von Twitter-Usern,
wenn sie über verschiedene Themen twittern? Finden sich verschiedene Netzwerk-Typen bei
Politik und beispielsweise Sport, je nachdem um was es in der Konversation gerade geht? Und
ist dies auch innerhalb eines Themas unterschiedlich, je nach benutztem Hashtag?
Um diese Fragen zu beantworten, habe ich eine Analyse von Twitter-Daten vorgenommen. Die
Daten wurden über die Twitter-API ausgelesen und mithilfe einer Netzwerkanalyse betrachtet.
So sollen Rückschlüsse auf die Gesamtheit der Twitter-User und eventuell auch auf die Natur des
generellen politischen Diskussion möglich werden.
Die untersuchten Hashtags wurden in die zwei Kategorien Politik und Sport eingeteilt, so geht es
einerseits um das Thema Griechenland und den Grexit, andererseits um die FIFAFrauenweltmeisterschaft in Kanada und die U21 Europameisterschaft in Tschechien.

# Grundlagen und bisherige Forschung

Die Forschung zur Polarisierung politische Diskurse reicht zurück bis in die 1940er Jahre. Bei
der Studie der Präsidentschaftswahlkampagne in den Vereinigten Staaten entdeckten die
Forscher, dass sich Rezipienten denjenigen Informationen eher zuwandten, die ihren vorher
geformten Einstellungen entsprachen. Über Festinger, Sunstein und weitere bekannte Autoren
gewann das Thema immer weiter an Brisanz, bis sich das Internet verbreitete und der These
noch einen weiteren Anstoß gab (Stroud 2014, 1).
Das Internet gilt als Medium mit der höchsten Auswahlmöglichkeit für den Rezipienten. In
keinem anderen Medium lassen sich die Informationen, die den Nutzer erreichen, so individuell
abstimmen. Sowohl im Fernsehen, im Radio als auch bei der Zeitung gibt es einen von der
Redaktion vordefinierten Informationsstrom, dem sich der Nutzer zwar zuwenden oder
abwenden kann, bei dem aber immer wieder Teile auftauchen, die er eigentlich vermieden hätte.
In diesem Sinne ist das Internet eine Bereicherung und Erleichterung für den Nutzer, stellt aber
für die Kommunikations- und Demokratietheorie einen unklaren Faktor dar. Viele gingen bisher
davon aus, dass beispielsweise ein gemeinsam geschautes Fernsehprogramm ein Level für den
politischen Austausch darstelle (Schulz 2011, 131).
Mit dem Internet und der Tendenz der politischen Polarisierung wird dieses gemeinsame Level
gefährdet. Es gibt jedoch Autoren wie Smith et al., die davon ausgehen, dass es mehr als nur
Polarisierung im Internet gibt. Insgesamt finden die Autoren sechs verschiedene
Interaktionsstrukturen. Neben dem klassisch polarisierten Bild können sich die Nutzer auch
einig sein, fragmentieren, clustern oder in Hubs Informationen erhalten oder ihre Verbreitung
unterstützen (Smith et al. 2014).
Zunächst gibt es die polarisierte Menge, ganz wie in der bisherigen Polarisierungsforschung
angenommen. Aber neben dieser Formation finden Smith et al. beispielsweise noch die „enge“
Menge, also eine Gemeinschaft, bei der sich keine klaren Untergruppen abgrenzen lassen.
Weiter geht es mit den „Brand Clusters“, die so heißen, weil sich Twitter-Nutzer, die sich über
VIPs oder Produkte verschiedener Firmen austauschen, selten untereinander austauschen
sondern eher auf die berühmte Person oder die Firma verweisen. Im Vergleich dazu fanden
Smith et al. auch „Community Clusters“, die ähnliche Strukturen ausbilden, aber sich viel öfter
untereinander austauschen, sodass stärkere Verbindungen zwischen den Mitgliedern der
Cluster entstehen. In den beiden letzten Netzwerkstrukturen gibt es jeweils einen zentralen
Akteur, der die Kommunikation mehr oder weniger bestimmt. Beim „Broadcast Network“5
sendet dieser zentrale Akteur Informationen an eine große Menge verbundener Nutzer, die die
Information weiter verbreiten. Im Gegensatz dazu interagiert der zentrale Akteur im „Support
Network“ mit den Nutzern, indem er auf ihre Nachrichten reagiert (Smith et al. 2014, 2-4).
Das Internet und Social Networking Dienste wie Facebook und Twitter eignen sich besonders
gut um Netzwerke zu analysieren. Musste man vorher umständliche Umfragen durchführen, um
soziale Netzwerke zu erforschen, wurden diese nun mithilfe der neuen Technologie sichtbar
gemacht (für die Beschreibung verschiedener Netzwerk-Arten siehe Newman 2003). Soziale
Netzwerke werden zu messbaren Energieimpulsen. Dabei muss man auf Twitter aber zwischen
mehreren Interaktionstypen der Nutzer unterscheiden, denn nicht jede Interaktion hat dieselbe
Bedeutung, dem sollte man sich bei der Analyse und der Interpretation bewusst sein.
Zum einen gibt es die Follower/Following-Beziehungen. Ist ein Nutzer der Follower eines
anderen, so bedeutet das, dass er an den Nachrichten des anderen interessiert ist und über
dessen Aktivitäten in seinem News-Feed informiert werden möchte. Der „gefolgte“ Nutzer hat
die Möglichkeit, seine Nachrichten direkt an seine Follower zu schicken und auf
schnellstmöglichem Wege zu verbreiten.
Zum anderen sind da noch die @messages oder auch @mentions. Mithilfe des @-Zeichens und
des Namens eines Nutzers lässt sich direkt auf einen Nutzer verweisen oder diesen in einem
Tweet erwähnen. Der erwähnte Nutzer sieht den Tweet, genauso wie dessen und die eigenen
Follower. Steht das @-Zeichen jedoch am Anfang eines Tweets, so handelt es sich um eine
Antwort (Reply) und wird den eigenen Followern und die des angesprochenen nicht direkt
angezeigt. Man benutzt @messages meist, wenn man jemanden darauf aufmerksam machen will,
was man getweetet hat, oder wenn ein Kommentar zu jemandem oder einem anderen
Kommentar inhaltlich in Verbindung steht.
Außerdem sind noch Retweets zu erwähnen. Bei einem Retweet übernimmt ein Nutzer den
Tweet eines anderen Nutzers mit der Notiz, von wem er den Tweet hat. Meist wird dadurch
Zustimmung ausgedrückt, da der Tweet fast wie ein eigener in die Tweet-Übersicht eingeht,
oder man möchte seine Follower auf diesen Tweet hinweisen (für mehr Informationen, Metaxas
et al. 2014).
Zusammen mit den Hashtags bilden diese Funktionen das Kerngerüst der Interaktion in
Twitter. Hashtags sind hierbei das Erwähnen eines bestimmten Schlagwortes oder eines
Themas, signalisiert durch ein vorangestelltes #-Zeichen. Das nachgestellte Schlagwort wird6
automatisch zum Link, unter dem sämtliche Tweets gesammelt werden, die ebenfalls das
Schlagwort mit #-Zeichen enthalten. So kann man auch auf andere Themen oder Nutzer
verweisen und seinen Tweet der ganzen Twitter-Öffentlichkeit (und nicht nur seinen eigenen
Followern) zugänglich machen.
Viele Forscher haben sich bereits damit beschäftigt und es hat sich gezeigt, dass es sich durchaus
lohnt, zwischen den verschiedenen Twitter-Funktionen in der Untersuchung zu unterscheiden
(Conover et al. 2012, Smith et al. 2014, Metaxas et al. 2014 etc.). So zeigen beispielsweise Smith
et al., dass in den einzelnen, in der Analyse gefundenen Nutzer-Cluster jeweils verschiedene
Hashtags und verschiedene Links benutzt wurden, um über das Thema zu sprechen.
Demokraten bezogen sich in der politischen Diskussion eher auf Mainstream-News-Webseiten,
während Republikaner vermehrt auf politische Kommentarseiten oder eher rechte NewsWebseiten verlinkten (Smith et al. 2014, 2). Conover et al. zeigten in ihrer Studie durchaus
polarisierte Kommunikationsstrukturen, die jedoch verschwanden, wenn man sich die FollowerBeziehungen ansah (Conover et al. 2012, 11). Dieses Ergebnis spricht dafür, dass die Nutzer
zwar polarisiert waren, wenn es darum ging Inhalte zu verbreiten und zu kommentieren, die
Interessen aber breit verteilt waren und sich die Nutzer mehreren, nicht unbedingt homogenen
Informationsquellen zuwandten. Metaxas und Mustafaraj zeigten ähnliche Ergebnisse, allerdings
bei einer Untersuchung der @messages (Jungherr 2014, 53).
Ich werde mich bei meiner Analyse auf den Vergleich von @mentions und Replies
konzentrieren, da Follower/Following-Beziehungen zu starr sind und sich nur schlecht nach
Themen einteilen lassen und die Hashtags eher dazu dienen, die Themen der Netzwerke
einzugrenzen und zu bestimmen, als diese abzubilden. Wie eben erwähnt, haben die Funktionen
unterschiedliche Bedeutung für den Nutzer. Während man bei einem Reply in einem inhaltlichen
Kontext antwortet, der manchmal für den Außenstehenden nicht so leicht erkennbar ist, so
lassen sich bei @messages eigene Gedanken und Kommentare anbringen und beliebige
Personen ansprechen, ohne dass bereits vorher eine Konversation zu dem Thema stattgefunden
haben muss.
Dieser kurze Überblick zeigt schon, dass die Ergebnisse durchaus variieren, je nach dem welches
Konzept man untersucht und auch in welchem Land man die Daten sammelt. Allein durch die in
jedem Land verschiedenen Wahl- und Mediensysteme können sich die Netzwerke
unterscheiden. Natürlich ist es mit vielen Hashtags klar, aus welchem Land der Großteil der
Tweets kommt (wenn man zum Beispiel Hashtags zur Präsidentschaftswahl nimmt), doch viele
Themen werden international diskutiert, sodass sich auch schlecht eine klare7
Parteizugehörigkeit zuordnen lässt. So betrifft zum Beispiel das Thema „Grexit“ vor allem die
Griechen, die auch aktiv darüber twittern, doch auch Nutzer aus den anderen europäischen
Staaten greifen das Thema auf und benutzen diesen zentralen Begriff. Es macht bei solchen
Themen wenig Sinn, die Analyse auf nur ein Land zu beschränken. Sehr wohl macht es aber Sinn,
zwischen verschiedenen Arten von Hashtags zu unterscheiden. Smith et al. haben bereits
gezeigt, dass unterschiedliche Themen angesprochen werden, je nachdem welche Gruppe von
Twitter-Nutzern untersucht wurde. Genauso werden auch die Nutzerstrukturen verschiedener
Hashtags nicht gleich aussehen.

# Netzwerkstrukturen im Vergleich

Ausgehend von dem eben erwähnten theoretischen Hintergrund, erwarte ich verschiedene
Ergebnisse, die ich in den Hypothesen zusammengefasst habe. Da es im Rahmen dieser Arbeit zu
aufwendig wäre, um speziell für die einzelnen Interaktionsstrukturen zu testen, die Smith et al.
vorschlagen, werde ich meine Erwartungen auf allgemeinere Gesichtspunkte beziehen
H1: Die Netzwerke von nicht-politischen Hashtags sind enger und dichter als die von
politischen Hashtags.
Ich denke, dass es bei politischen Hashtags eher zu Cluster- oder Gruppen-Bildung kommt, da
die diskutierten Themen ein größeres Potential bieten, sich differenziert darüber
auszutauschen. Es könnte thematische oder ideologische Cluster geben, beispielsweise je nach
Parteizugehörigkeit. Je nachdem wie dieses Cluster aussieht, könnte man auch wieder eine
polarisierte Diskussion vermuten.
H2: Reply-Netzwerke sind fragmentierter und weniger dicht als @mention-Netzwerke.
Wie schon erwähnt, bilden Replies eine Untergruppe der @mentions und finden meist in einem
bestimmten Kontext statt, nämlich in dem Wissen um die Nachricht, auf die von einem Nutzer
geantwortet wurde. Demnach sind diese Interaktionen meist in sich geschlossen und weniger
untereinander verknüpft.

# Ursprung des Datensatzes

Meine Analyse möchte ich im Folgenden auf die Hashtags #grexit, #greferendum, #varoufakis
und #greece sowie #U21euro, #fifawwc, #GERFRA und #PORGER beschränken, die ebenfalls
international diskutiert wurden.8
#grexit bezieht sich auf den möglichen Austritt Griechenlands aus der Eurozone, die
Entscheidung dieser Situation stand zum Zeitpunkt der Analyse kurz bevor. Die meisten Tweets
mit dem Hashtag #greece bezogen sich ebenfalls auf dieses Thema. #greferendum wurde das
geplante Referendum in Griechenland genannt, bei dem die Griechen selbst entscheiden sollten,
ob sie den Forderungen der anderen Euro-Länder nachkommen wollten. Das Thema wurde in
den Medien sehr kontrovers diskutiert und war auch in Twitter ein populäres Thema. Yanis
#varoufakis ist der Name des ehemaligen Finanzministers der Griechen. Er war die Person, die
sich am meisten und am öffentlichsten gegen die Reformvorhaben aussprach und über den auch
oft getwittert wurde.
Zur gleichen Zeit fanden auch zwei sportliche Großereignisse statt, zum einen die FIFA
Frauenweltmeisterschaft in Canada, zum einen die U21 Europameisterschaft in Tschechien.
Dazu werde ich die passenden Hashtags untersuchen, einmal #fifawwc (FIFA Women World
Championship) und #U21euro. Bei diesen sportlichen Ereignissen war jeweils ein Spiel aktuell,
dass sowohl deutschlandweit als auch international kommentiert wurde. Die deutsche
Frauennationalmannschaft spielte erfolgreich gegen Frankreich (#GERFRA), die U21-Herren
verloren katastrophal gegen die Portugal-U21 (#PORGER).
Diese Hashtags waren am Samstag, dem 27. Juni 2015, alle aktuell und teilweise in den TwitterTrends. Im Anschluss wurden die Nutzer untersucht, die diese Hashtags verwendeten, und wie
sie in ihrem Netzwerk positioniert sind, also wem sie @messages schrieben oder auf welche
Tweets sie antworteten. Von diesen acht Hashtags wurden 200 Tweets über die Twitter-API
abgerufen, die neue und populäre Tweets gemischt enthielten. Um nun herauszufinden, in
welchen Netzwerkstrukturen diese Hashtags benutzt wurden, dienten die Autoren der 200
Tweets als Ausgangslage, um über deren Timeline die Interaktionsstrukturen zu untersuchen.
Um die praktische Durchführbarkeit der Analyse zu gewährleisten, wurde die Anzahl der zu
untersuchender Tweets auf 20 pro Nutzer beschränkt. Bei einer umfassenderen Analyse kann
und sollte dieser Wert hochgesetzt werden.
Die nun gesammelten Tweets wurden auf Interaktionsmerkmale untersucht, also jeweils auf
@mentions und Replies, wobei klar separiert wurde, um die Ergebnisse später vergleichen zu
können. Dabei stellten die Replies logischerweise eine Untergruppe der @mentions dar, worauf
ich aber später noch einmal zurück kommen möchte.
Mithilfe des Programms NodeXL war es schließlich möglich, die stattgefundenen Interaktionen
in einer Netzwerkstruktur darzustellen und zu analysieren. Zusätzlich wurde über Wakita-9
Tsurumi-Algorithmus die Anzahl und Ausprägung möglicher Gruppen berechnet, die in den
Grafiken farbig dargestellt werden.
Mein Skript basiert dabei vor allen Dingen auf den Skripten aus dem Kurs und eigenen
Bearbeitungen, wo sie benötigt wurden.

# Auswertungder Daten

## H1: Die Netzwerke von nicht-politischen Hashtags sind enger und dichter als die von
politischen Hashtags.

Wenn man sich die Netzwerke der einzelnen Hashtags betrachtet, sieht man gleich Unterschiede
zwischen den Themengebieten Politik und Sport. Abbildung 1 zeigt die Verteilung beim Hashtag
#greferendum, während Abbildung 2 #fifawwc zeigt. Man sieht deutlich, dass der Hashtag aus
dem Themenbereich Politik ein dichteres und engeres Netzwerk abzubilden scheint als der
Hashtag aus dem Themengebiet Sport. 

![png](/img/2015/06/abb1-abb2.png)

Dieser Eindruck setzt sich auch fir die Netzwerke der anderen Hashtags fort, je nachdem
welchem Themengebiet sie zuzuordnen sind. Bei sportlichen Hashtags kommt es auch öfter vor,
dass manche Untergruppen
komplett losgelöst vom
restlichen Netzwerk dargestellt
werden (siehe Abbildung 3,
#PORGER).
Diese Ergebnisse sind auch in
den berechneten Metriken
nachzuvollziehen. Die Dichte
(Density)[1], also der Koeffizient,
der berechnet wie stark die
einzelnen Netzpunkte
miteinander verknüpft sind (Newman 2003, 12), ist bei den Sport-Hashtags durchgehend höher
als bei den Politik-Hashtags (siehe Tabelle 1). Das bedeutet, dass diese Netzwerke stärker in sich
selbst vernetzt sind. Dagegen ist auch der Wert des Geodesic path (Diameter) höher. Dieser
Diameter gibt an, wie viele Schritte notwendig sind, um von einem Punkt des Netzwerkes zum
anderen zu gelangen (Newman 2003, 5). Das bedeutet, dass das Sport-Netzwerk zwar mehr
Verbindungen hat, aber insgesamt weniger in sich verbunden ist, da in den Politik-Netzwerken
die kürzesten Wege im Vergleich kürzer sind.
Ein weiterer auffälliger Wert ist der der „Connected Components“ (siehe Tabelle 1). Der Wert
gibt an, wie viele Punkte untereinander verbunden sind, jedoch nicht mit dem Rest des Graphen.
Vergleicht man nun die Ausprägungen für Sport und Politik sieht man, dass die Politik-Hashtags
zwar durchweg mehr Vertices aufweisen, aber der Wert der Connected Components niedriger
ausfällt, sie also weniger losgelöste Teil-Netzwerke haben.

Auch die Art der Teilgruppen variiert deutlich zwischen den einzelnen Netzwerken, auch wenn
hier keine Regelmäßigkeit beim Vergleich von Sport- und Politik-Hashtags festgestellt werden
konnte. Am Beispiel des Netzwerks für #fifawwc sieht man deutlich, dass es einerseits mehr
oder weniger zentralisierte Untergruppen gibt (siehe Abbildung 4), andererseits aber auch weit
verzweigte (siehe Abbildung 5).

## H2: Reply-Netzwerke sind fragmentierter und weniger dicht als @mentionNetzwerke.

Im Vergleich der zwei untersuchten
Interaktionsstrukturen sieht man auch gleich
Unterschiede. Die Netzwerke der Replies sind
weniger dicht und generell kleiner, da sie ja
auch ein Teilnetzwerk der @mentionNetzwerke sind. Typisch für die ReplyNetzwerke sind einzelne sternförmige
Interaktionen, bei denen zum Beispiel ein
Nutzer auf mehrere andere Tweets antwortete,
oder mehrere Nutzer auf einen besonderen
Tweet reagierten (siehe Abbildung 7).

# Schlussbetrachtung

Alles in allem zeigt die vorliegende Analyse, dass es sehr wohl Unterschiede zwischen den
Netzwerken verschiedener Hashtags und Themen gibt. Mit einer erweiterten Analyse könnte
man noch mehr über die Eigenschaften dieser Netzwerke sagen und wie sie sich im Laufe der
Zeit verändern. Dann könnte man eventuell für jedes Thema oder für jeden Themenbereich eine
exakte Zuordnung zu den sechs Typen von Smith et al. vornehmen und vielleicht auch
Vorhersagen wagen, wie sich ein Hashtag in Zukunft entwickeln wird. Mit der geeigneten
Hardware ließen sich die Zahlen der gesammelten Tweets vervielfachen, und nach dem Gesetz
der großen Zahlen, werden solche Modelle und Vorhersagen immer genauer, je mehr Daten in
die Analyse aufgenommen werden können. Zum Beispiel könnte untersucht werden, ob es
zentrale Akteure in den Netzwerken gibt, immer wiederkehrende Informationskanäle, oder
konversationsbestimmende "Influencers", wie Smith et al. sie nennt (Smith et al. 2014, 7).
In einem zweiten Schritt wäre es sicherlich auch interessant, der aktuellen Forschung zu folgen
und zu untersuchen, ob und wie sich die gezeigten Cluster auch inhaltlich voneinander
unterscheiden. Dies wäre beispielsweise durch eine Funktion von NodeXL machbar, mit der
automatisch Tweets gezogen und in verschiedenen Kategorien gemessen werden, unter
anderem Top Domains, Top Hashtags, Top Words und Top Mentions.
Es wäre beispielsweise interessant zu sehen, ob sich die Twitter-Nutzer zwar in Clustern
bewegen, aber innerhalb dieser Gruppen trotzdem über verschiedene Themen kontrovers
sprechen. Somit würde zwar die Struktur des Netzwerkes für eine fortschreitende
Fragmentierung des Diskurses sprechen, aber der Inhalt des Diskurses könnte aufkommende
Sorgen wie von Sol Stein zerstreuen.
Eine weitere Möglichkeit diese Analyse auszuweiten wäre, die Messung an mehreren
Zeitpunkten fortzusetzen, um so zu erfahren, ob sich die Interaktionsstruktur zu einem Hashtag
über die Zeit verändert, oder ob sie gleich bleibt. Hashtags durchlaufen immer eine Karriere, von
dem Punkt an dem sie populär werden bis sie wieder an Aktualität verlieren. Diese Karriere
schlägt sich mit Sicherheit auch in den Netzwerkstrukturen sichtbar nieder.
Auf jeden Fall bleibt bei allen Forschungsmethoden immer zu beachten, dass man einen
Kurznachrichtendienst untersucht. Reale Freundschaftsverhältnisse und Interessensneigungen
werden vielleicht teilweise abgebildet, aber eben mit Sicherheit nicht komplett. Man sollte nicht
den Fehler machen und die Ergebnisse eins zu eins auf die reale Welt übertragen. Dennoch lässt14
sich eventuell aus diesen Ergebnissen einiges schließen, vor allem wenn man bedenkt, dass es
immer mehr Leute gibt, die sich nur noch über das Internet über aktuelle Geschehnisse
informieren (für eine Diskussion der Übertragbarkeit siehe Metaxas/Mustafaraj 2012).
In Zukunft bleibt nur zu wünschen, dass die aktuelle Forschung etwas zur Einheitlichkeit und
Vergleichbarkeit der Studien beiträgt, damit eine nicht ganz so große Unsicherheit darüber
besteht, welche Methode und welche Operationalisierung am sinnvollsten sind, und was dies im
Vergleich mit anderen Studien bedeutet.

# Appendix

## [Python Code](/blog/cluster-python-code)

# Fußnoten

[1] Die Dichte (Density) wird bei NodeXL durch den Vergleich der tatsächlichen Anzahl der Kanten mit der
höchstmöglichen Anzahl der Kanten, wenn alle Eckpunkte miteinander verbunden wären, berechnet.

# Literatur

Conover, Michael D./Bruno Goncalves/Alessandro Flammini/Filippo Menczer. 2012. “Partisan
asymmetries in online political activity.” EPJ Data Science 1: 1-19.
Farrell, Henry. 2012. “The Consequences of the Internet for Politics.” Annual Review of Political
Science 15: 35-52.
Gentzkow, Matthew und Jesse M. Shapiro. 2011. “Ideological Segregation Online and Offline.” The
Quarterly Journal of Economics 126, 1799-1839.
Jungherr, Andreas. 2014. “Twitter in Politics: A Comprehensive Literature Review.” Social
Science Electronic Publishing. http://ssrn.com/abstract=2402443, letzter Zugriff am 11.07.2015
Lawrence, Eric, John Sides und Henry Farrell. 2010. “Self-Segregation or Deliberation? Blog
Readership, Participation, and Polarization in American Politics.“ Perspectives on Politics 8 (1):
141-157.
Metaxas, Panagiotis T./Eni Mustafaraj. 2012. “Social Media and the Elections.” Science 338: 472-
473.
Metaxas, P. T./E. mustafaraj/K. Wong/L. Zeng/M. O’Keete/S. Finn. 2014. “Do Retweets indicate
Interest, Trust, Agreement? (Extended Abstract).” http://arxiv.org/abs/1411.3555, letzter
Zugriff am 11.07.2015.
Newman, M. E. J. 2003. “The structure and function of complex networks.” SIAM Review 45 (2):
167-256.
Schulz, Winfried. 2011. Politische Kommunikation. Theoretische Ansätze und Ergebnisse
empirischer Forschung. 3. Auflage. Wiesbaden: Springer VS.
Smith, Marc A./Lee Rainie/Itai Himelboim/Ben Shneiderman. 2014. “Mapping Twitter Topic
Networks. From Polarized Crowds to Community Clusters.” PewResearchCenter.
http://www.pewinternet.org/2014/02/20/mapping-twitter-topic-networks-from-polarizedcrowds-to-community-clusters/, letzter Zugriff am 11.07.2015.
Stroud, Natalie Jomini. 2014. Selective Exposure Theories. The Oxford Handbook of Political
Communication (Forthcoming). Oxford: Oxford University Press.