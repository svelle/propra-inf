title: Git Branches
stage: alpha
timevalue: 2.0
difficulty: 2
assumes: git-Funktionsweise
requires: git-Zweitrepo
---

[SECTION::goal::idea]

Ich verstehe wie Branches in git funktionieren, wann es sinnvoll ist, Branches zu verwenden und 
welche Probleme dabei entstehen können.

[ENDSECTION]

[SECTION::background::default]
Branches stellen einen der wichtigsten Grundbausteine für Git dar, diese zu verstehen ist 
essenziell um zu verstehen wie git funktioniert und wie man damit besonders Produktiv ist. In 
diesem ersten Teil beschäftigen wir uns in erster Linie mit der Theorie hinter git-Branches, 
danach schauen wir uns an wie man damit in der Praxis arbeitet.
[ENDSECTION]

[SECTION::instructions::detailed]

### Was ist ein Git-Branch?

Wie eingangs erwähnt, ist es essenziell zu verstehen, was Branches in Git sind und wozu man sie 
verwendet. Doch was genau ist eigentlich ein Branch?

#### **Definition eines Git-Branches**

Einfach gesagt, ist ein Branch nur ein Zeiger auf einen bestimmten Commit.  
Das klingt zunächst sehr simpel, oder? Schließlich stellt man sich unter einem „Branch“ aufgrund 
des Begriffs typischerweise eine Abzweigung vor, wie bei einem Baum mit Ästen und Zweigen. Diese 
Metapher funktioniert aber nur teilweise – warum das so ist, klären wir gleich noch. Doch 
zunächst zurück zur Theorie

Um das Konzept von Git-Branches besser zu verstehen, lesen Sie den Abschnitt [Git Branches in a 
Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) bis zum Abschnitt 
**„Creating a new Branch“** und beantworten Sie anschließend folgende Fragen:

- [EQ] Was ist ein Git-Branch?  
- [EQ] Ist `master` bzw. `main` ebenfalls ein Branch?
- [EQ] Aus der theoretischen Informatik kennen wir einige Konzepte der Grafentheorie, mit 
  welchem davon lässt sich das Konzept von Git-Branches gut beschreiben?

### Praktische Nutzung von Git-Branches

Nun, da wir wissen, was ein Git-Branch in der Theorie ist, können wir uns der Frage widmen, wozu 
Branches praktisch genutzt werden.  

Zu Beginn haben wir erwähnt, dass die Baum- bzw. Zweigmetapher nur bedingt funktioniert. Der 
Grund dafür ist, dass Branches zwar wie Zweige von einem Hauptstamm abzweigen können, Änderungen 
jedoch auch wieder in den Haupt-Branch (z. B. `main` oder `master`) integriert werden. Dafür 
stehen verschiedene Methoden zur Verfügung, z. B. **Merging** und **Cherry-Picking**. Wie diese 
im Detail funktionieren lernen wir später anhand einiger praktischer Beispiele.

#### Warum mit Branches arbeiten?

Die Arbeit mit Branches bietet sowohl in Solo-Projekten als auch in der Teamarbeit viele Vorteile.  

Grundsätzlich gibt es keine starren Regeln, wann Branches verwendet werden müssen oder sollten. 
Dennoch kann die Nutzung von Branches viele Probleme vermeiden, vor allem bei der Zusammenarbeit 
in Teams. Durch Branches können Änderungen isoliert werden, ohne die Stabilität des 
Haupt-Branches zu gefährden.

### Der Feature-Branch

Schauen wir uns ein einfaches Szenario an, das in nahezu jedem Projekt sinnvoll genutzt werden 
kann: den sogenannten Feature-Branch.

Wenn wir Software entwickeln, ist es gängige Praxis, für neue Features einen eigenen Branch zu 
erstellen. Doch warum sollten wir das tun?  

Zunächst ermöglicht es uns, sorglos zu arbeiten und zu experimentieren. Wir können Änderungen 
vornehmen, so viel wir wollen, ohne befürchten zu müssen, dass unser Haupt-Branch verändert wird.
Insbesondere bei der Arbeit im Team hilft ein Feature-Branch, Konflikte zu vermeiden und 
sicherzustellen, dass die Arbeit anderer Teammitglieder nicht gestört wird.

Wenn wir mit unserem Feature zufrieden sind, können wir den Feature-Branch wieder in den 
Haupt-Branch integrieren – meist durch einen sogenannten **Merge** oder **Pull Request**.

Hier kommt die Baum-Metapher jedoch an ihre Grenzen: In der Natur wächst ein Zweig schließlich 
nicht wieder in den Stamm zurück. In der Softwareentwicklung hingegen können Änderungen aus 
Zweigen (Branches) zurück in den Haupt-Branch geführt werden, wodurch der Projektstamm 
stetig wächst und dabei aber stabil bleibt.

--- insert task

Fragen:

- Ist ein feature branch eher lang oder kurzlebig, welche probleme können dadurch auftreten?

### Der Release-Branch

Angenommen, wir haben ein Git-Repository für eine Software, die einen stabilen Zustand erreicht 
hat und veröffentlicht werden soll – beispielsweise als Version 1.0.  

Um diesen Zustand dauerhaft festzuhalten, können wir einen neuen Branch namens **release-1** 
erstellen. Dieser Branch verweist fortan auf den Commit, der die Version 1.0 repräsentiert.

Warum nicht einfach den Commit auschecken?

Auf den ersten Blick mag es unnötig erscheinen – schließlich könnte man auch einfach den 
entsprechenden Commit auschecken, um den Code später erneut einzusehen oder Änderungen 
vorzunehmen. Doch was passiert, wenn wir diese Änderungen committen wollen?  

Die Antwort: Die Git-Historie kann nicht ohne Weiteres geändert werden. Ein Commit in einem 
früheren Zustand würde bedeuten, dass wir die bestehende Historie umschreiben müssten – was in 
der Regel vermieden werden sollte, um Konsistenz und Nachvollziehbarkeit zu gewährleisten.

Die Lösung: Arbeiten im Release-Branch

Statt direkt im Haupt-Branch Änderungen vorzunehmen, nutzen wir den Release-Branch:  
- Dieser Branch kann problemlos ausgecheckt werden, um Anpassungen wie Bugfixes oder 
  Sicherheitsupdates vorzunehmen.  
- Änderungen werden im Release-Branch committed und anschließend gepusht.  
- Danach lässt sich der Haupt-Branch wieder auschecken, um dort an neuen Features oder 
  Verbesserungen weiterzuarbeiten.

Mit dieser Vorgehensweise bleibt der Haupt-Branch stabil und sauber, während für wichtige 
Änderungen in veröffentlichten Versionen eigene Branches existieren.


Nun wollen wir das mal in der Praxis ausprobieren.

1. Erstellen Sie neues Verzeichnis und initialisieren Sie darin ein neues Git-Repo.
2. Legen Sie eine neue Datei an wie z.B. **main.c** und geben Sie ihr einen halbwegs sinnhaften 
   Inhalt 
   (Hello World)
3. Commiten Sie die Datei in ihren Haupt-Branch
4. Legen Sie nun einen neuen Branch an, **release-1**. Diesen brauchen Sie noch nicht auszuchecken.
5. Nehmen Sie nun ein paar Änderungen am Haupt-Branch vor. Fügen Sie z.b. eine neue Datei hinzu 
   und Commiten Sie diese in diesen Branch.
6. Als nächstes Nehmen Sie eine Änderung an der **main.c** Datei vor und erzeugen Sie auch 
   hierfür wieder einen Commit. Das ist nun unser "Patch".
7. Jetzt geht es darum, diese Änderungen in unseren Release-Branch einzupflegen.
8. Checken Sie also nun den release Branch aus und nutzen Sie dann `git cherry-pick` um den 
   *Patch-Commit* von unserem Haupt-Branch in den Patch-Branch zu bringen.

#### Release-Tags

Zusätzlich zu Branches gibt es in Git auch sogenannte **Tags**. Genau wie Branches sind auch 
Tags lediglich **Zeiger auf Commits**. Sie repräsentieren jedoch einen speziellen Punkt in der 
Historie, der typischerweise wichtig und unveränderlich ist, wie etwa ein **Release** oder ein 
bedeutender Meilenstein in der Entwicklung.

Fragen:
- Wann sollte man Tags statt Branches nutzen?
- Welche Vorteile bieten Tags gegenüber Branches?
- Wie erstellt man einen tag in einem git repo?
- Worin unterscheiden sich annotierte Tags von nicht-annotierten Tags?


### Feature-Flag bzw. Trunk-Based Development

*notes*
- unterschiedliche definitionen, short-lived branches into main oder feature flags im main branch
- feature flag im main branch nicht so beliebt wie short-lived branches da schwieriger für code 
  review

Nun haben wir ja schon einiges über Branch-Basierte Entwicklung gelernt, allerdings gibt es 
schon seit vielen Jahren auch eine andere Art der Entwicklung, welche zwar die Grundlegenden 
konzepte eines Branches übernimmt, jedoch dabei nicht auf die git implementierung zurückgreift, 
sondern diese im Programmcode abbildet.

Diese Art der Entwicklung wird häufig Feature-Flag bzw. Trunk-Based Development genannt.



[ENDSECTION]

[SECTION::submission::trace]

[INCLUDE::../../_include/Submission-Kommandoprotokoll.md]

[ENDSECTION]

[INSTRUCTOR::Befehle prüfen, und ob Branching und die Behebung von Merge-Conflicts verstanden wurde]

[INCLUDE::ALT:]

[ENDINSTRUCTOR]
