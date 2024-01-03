# Bresser CO2-Messgerät mit Datenlogger zur Prüfung der Luftqualität INV 

Das Bresser CO2-Messgerät mit Datenlogger zur Prüfung der Luftqualität INV wurde von Bresser bis Anfang Dezember 2023 für faire 59 € verkauft https://web.archive.org/web/20231124115239/https://www.bresser.de/Wetter-Zeit/Raumklima-und-Luftqualitaet/BRESSER-CO2-Messgeraet-mit-Datenlogger-zur-Pruefung-der-Luftqualitaet-INV.html 
Aktuell möchte Bresser 189 €, was überteuert ist.

Dieses Repository dient dazu alles zu sammeln, was über dieses Messgerät bekannt ist.

Man sollte die Anleitung von https://www.bresser.de/out/media/f89150513b55e2f2856d9fe829e43dc3.pdf  herunterladen. Diese ist viel detaillierter als die mitgelieferte, obwohl sie auf den ersten Blick gleichaussieht. In der PDF-Variante ist z.B. der Kalibrierungsprozess beschrieben, welcher in der gedruckten Ausgabe fehlt.

Die Uhrzeit braucht man nicht am Messgerät einstellen, da man diese bequem über die Software setzen kann.

Das CO2-Messgerät meldet sich bei Windows mit einer HID-Schnittstelle, ähnlich des TFA AirCO2ntrol Mini, siehe https://github.com/JsBergbau/TFACO2AirCO2ntrol_CO2Meter
Über einen ähnlichen Weg sollte man hier die Daten live auslesen können und auch herunterladen können. Wer sich daran machen möchte, gerne einen Pullrequest erstellen.

Mit der mitgelieferten Software http://archive.bresser.de/download/7004040/Software/Software_HT-Communication-Tool.zip für Windows kann man die aktuellen Werte live am PC anzeigen und die Aufzeichnung konfigurieren und die Daten herunterladen. Abgespeichert werden die Werte in einem proprietären binären Format. Dieses kann man als Excel-Datei im XLS-Format (nicht XLSX), sowie als TXT exportieren. Dort sind dann oben die aktuellen Einstellungen und weiter unten die Daten im CSV-Format. Damit lässt sich arbeiten. 

Das Datenformat verfügt über keine Prüfsumme. Die Werte können beliebig manipuliert und wieder eingelesen werden, was das Reverseengineering erleichtert. Der Taupunkt wird wahrscheinlich aus Luftfeuchtigkeit und Temperatur berechnet. Es befinden sich insgesamt 3 Beispieldateien im Repository. Zwei bei denen nur ein Messwert gespeichert ist und eine mit 22 Messwerten. 
Bei der Datei 1Datenpunkt1.HTRec mit nur einem Messwert sind die letzten 2 Bytes `3B 04` der ppm Wert von 1083. Es ist ein INT16 im Little Endian Format. Hilfreich zur Analyse ist https://www.scadacore.com/tools/programming-calculators/online-hex-converter/

Der Wert 7C gibt die Temperatur an. Wie man auf diese kommt ist mir unklar. Da es nur ein Byte ist, scheint es nur 256 Werte zu geben. Jede Erhöhung oder Erniedrigung ergibt 0,1 °C Temperaturdifferenz. Eine Änderung am Byte vorher bewirkt keine Änderung der Temperatur. `00` ergibt 11,2 °C, `FF` 36,7 °C. 

`BF 12` ergibt eine Luftfeuchtigkeit von 44,7 %. 

`12` ist eine Art Skalierungsfaktor. Ändert man diesen, ändert sich sowohl die Temperatur, als auch die Luftfeuchtigkeit.

Freue mich, wenn hier weitere Details zusammengetragen werden, um dann z.B. ein Python-Skript zu erstellen.
