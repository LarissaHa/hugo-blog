+++
author = "Larissa"
categories = ["Python", "Tutorial"]
tags = ["spring2019", "django", "webdev", "text-adventure", "german-only"]
date = "2019-03-16"
description = "Schattengabe - Tutorial Part 01"
featured = "forest.jpg"
featuredpath = "date"
linktitle = ""
title = "How to build a Text Adventure Game"
type = "post"
+++

In den letzten Wochen und Monaten habe ich ein paar Projekte mit Django, dem Web Framework basierend auf Python, gearbeitet. Ich muss zugeben, am Anfang fand ich das alles ziemlich kompliziert. Django verlangt einen Berg an verschiedenen Dateien, die alle ineinander greifen, und wenn man an der einen Stelle etwas ändert, zieht das meistens dreimal soviele notwendige Änderungen in anderen Dateien mit sich. Je mehr ich allerdings mit dem System arbeitete und je mehr Seiten ich damit baute, desto verständlicher und intuitiver wurde für mich alles. Und nach und nach entdecke ich auch die vielen Möglichkeiten, die Django mit dem machtvollen Python im Hintergrund bietet.

# Text Adventure

Das neue Projekt, ein Text Adventure Spiel, soll auch wieder mit Django gebaut werden. Zur Erklärung: Das Spiel soll zum Schluss mehrere Episoden an Text-Abschnitten mit anschließenenden Aufgaben bieten. Die Aufgaben können Rätsel sein, Antworten auf Fragen, oder Bilder, die an bestimmten Orten aufgenommen werden sollen. Das Lösen der Aufgaben ist Voraussetzung für das Freischalten weiterer Episoden. Bestenfalls gibt es im Hintergrund noch ein Punkte- und Level-System sowie einen Entscheidungsbaum, der die Entscheidungen der Spieler speichert und nachvollziehbar macht und irgendwann das Spiel zu einem individuellen Erlebnis werden lässt.

# Mit Django beginnen

Zu allererst möchte ich auf die Seite von [Django Girls](https://tutorial.djangogirls.org/de/) verweisen, die ein super Tutorial zur Verfügung stellen, wie man Django from scratch auf seinem Rechner zum Laufen bringt. Zusätzlich erklären sie auch in einem angenehmen Tempo die ersten Deploying-Schritte mit Git, GitHub und [pythonanywhere](http://pythonanywhere.com). So kann man seine Django-Versuche gleich im Internet zur Schau stellen! Ich kann jedem nur empfehlen, das Tutorial dort durchzuspielen, ich könnte es nicht besser erklären als dort.

# Änderungen fürs Text Adventure

Für unser Text Adventure "Schattenwege" habe ich bisher folgende Anpassungen getätigt (in der [models.py](https://github.com/LarissaHa/projx/blob/master/story/models.py)):

## Quest

{{< highlight py3 >}}
    #models.py
class Quest(models.Model):
    title = models.CharField(max_length=200)
    text = models.TextField()
    level = models.IntegerField()
    required = models.ForeignKey('self', on_delete=models.DO_NOTHING, blank=True, null=True)
    task = models.TextField()
    solution = models.CharField(max_length=100)
    key = models.CharField(max_length=200)

    #published_date = models.DateTimeField(blank=True, null=True)

    #def publish(self):
    #    self.published_date = timezone.now()
    #    self.save()

    def __str__(self):
        return self.title
{{< / highlight >}}

Die Episoden sind als Quests in der Datenbank angelegt. Jede Quest hat einen Titel und einen Text, das sind die klassischen Story-Elemente. Zusätzlich gibt es ein Level, das der Spieler haben muss, um die Quest zu spielen, und ein required-Feld, das einen Foreign Key zu einer anderen Quest enthält, die storytechnisch vorgelagert ist und neben dem Level ebenfalls vorausgesetzt wird. Außerdem kommen noch die Aufgabe (also die Frage, die Aktionsaufforderung), die Lösung (also das, wogegen die Eingabe abgeglichen werden soll), sowie der Schlüssel hinzu (was man erhält, wenn man die Quest löst). Der Schlüssel kann, muss aber nicht, Hinweise enthalten, die für das Lösen späterer Quests wichtig, wenn nicht sogar notwendig ist. (Die auskommentierten Teile sollen später erst aktiviert werden.)

## Spieler

Der Spieler hat im Moment nur einen Namen und ein Level. Geplant sind noch zusätzliche Felder wie Punkte, Titel, ein Avatar etc., aber das sind "Spielereien", die später angegangen werden sollen.

{{< highlight py3 >}}
    #models.py
class Player(models.Model):
    name = models.CharField(max_length=20)
    #avatar
    level = models.IntegerField()
    #add fields

    #def new_player(self):
    #    self.level = 0
    #    self.save()

    def __str__(self):
        return self.name
{{< / highlight >}}

## Gelöst-Tabelle

Wichtiger Baustein für die Funktion des Text Adventures bildet die Tabelle "Gelöst". Hier werden mit Datum die Quests gespeichert, die ein Spieler richtig löst. Auf diese Tabelle soll immer wieder zugegriffen werden, wenn es um das Freischalten von Quests geht.

{{< highlight py3 >}}
    #models.py
class Solved(models.Model):
    quest = models.ForeignKey(Quest, on_delete=models.DO_NOTHING)
    player = models.ForeignKey(Player, on_delete=models.CASCADE)
    solved_date = models.DateTimeField(blank=True, null=True)

    def solve(self):
        self.solved_date = timezone.now()
        self.save()
{{< / highlight >}}

## Views

Die Views, die dazu da sind, um die Inhalte für jede Seite zu generieren, bestehen im Moment fast nur aus simplen Return-Anweisungen. Nur die Quest-Seiten enthalten im Moment ein Eingabefeld, das die eingegebene Lösung gegen die Musterlösung in der Datenbank abgleicht. Ist die Anwort korrekt, wird der Schlüssel ausgegeben.

{{< highlight py3 >}}
    #views.py
def quest(request, pk):
    quest = get_object_or_404(Quest, pk=pk)

    solution = request.GET.get('q', '')
    if solution == quest.solution:
        key = ['x']
    else:
        key = []

return render(request, 'story/quest.html', {'quest': quest, 'key': key, 'solution': solution })
{{< / highlight >}}

Hier ist eventuell geplant, die eingegebene Lösung mit NLP-Verfahren zu normieren, um Unterschiede bei der Schreibweise, Groß- und Kleinschreibung, die Wortreihenfolge etc. als mögliche Fehlerquellen abzufangen. 

# Design

Das momentane Grunddesign für unsere Text Adventure Seite bildet [Endgam](https://colorlib.com/wp/template/endgam/) von colorlib. In meinen Augen ist dies ein entscheidender Vorteil von Django Seiten: Man kann alle HTML-Templates einbauen, die man im Internet so findet (oder sich selbst schreibt), und das sind verdammt viele. Es müssen nur wie gewohnt die CSS-Dateien im Template verknüpft werden, sowie die Templates mit den URLs und den richtigen Views.

# Stand Momentan

Die oben beschriebenen Änderungen sind zur Zeit Stand der Dinge. In den kommenden Wochen möchte ich nach und nach weitere Änderungen anzubringen und hier zu veröffentlichen. Eventuell ist auch ein kleines Tutorial zu Chatbots geplant, denn dieser soll als "NPC" unser Text Adventure interaktiver machen. 

Die Dateien sind einsehbar in meinem [GitHub Profil](https://github.com/LarissaHa/projx)