# Kuźnia — Android / Kotlin

Natywna aplikacja Jetpack Compose na podstawie przesłanego promptu i planszy ekranów. Minimum Android 8.0 (API 26). Dane są lokalne; aplikacja nie wymaga konta ani połączenia z serwerem.

## Czysta wersja na telefon — 0.1.35

Nowa instalacja rozpoczyna się bez planów, historii, imienia i zdjęcia. Generator przykładowych treningów oraz plany testowe znajdują się wyłącznie w źródłach testów i nie trafiają do APK. W Profilu ustaw imię, następnie utwórz pierwszy plan. Aktualizacja istniejącej instalacji zachowuje jej dane.

## Uruchomienie

1. W Android Studio wybierz **Open** i wskaż cały folder `kuźnia` (ten z `settings.gradle.kts`).
2. W ustawieniach Gradle wybierz JDK z Android Studio (Embedded JDK, minimum 17).
3. Poczekaj na synchronizację Gradle. Projekt używa AGP 9.1.1, Gradle 9.3.1, Kotlin/Compose 2.2.10 i SDK 37.
4. Wybierz emulator albo telefon z debugowaniem USB i uruchom konfigurację **app**.

`local.properties` wskazuje lokalne SDK i nie jest plikiem do przenoszenia na inny komputer. Android Studio utworzy go ponownie.

Folder zawiera polski znak. `android.overridePathCheck=true` pozwala budować projekt w tej lokalizacji, a `-Dfile.encoding=COMPAT` rozwiązuje problem kodowania ścieżki przy uruchamianiu testów przez Javę na Windows. Przy zmianie konfiguracji systemu najprostsza alternatywa to folder bez polskich znaków, np. `C:\projekty\kuznia`.

## Funkcje

- Cztery zakładki: Trening, Plany, Dziennik, Profil; polski interfejs, ciemna paleta i pomarańczowe akcenty z logo.
- Pusta lista planów przy pierwszym uruchomieniu. Dodawanie, usuwanie i edycja planów, ćwiczeń, serii, sprzętu i przerw. Zmiana kolejności strzałkami.
- Wybór mięśni i podgląd regeneracji korzystają z oryginalnego pliku „Schemat mięśni z przodu i z tyłu.png”. Nakładki kolorują szare obszary mięśni, zachowując kontur i odstępy na grafice. Można dotykać sylwetki albo używać przycisków z nazwami.
- Opcjonalny tygodniowy harmonogram planu: wybierz dowolne dni w edycji planu. Zakładka Trening wyróżnia plany przypisane do bieżącego dnia według lokalnej daty urządzenia. Gdy pasuje kilka planów, wszystkie są wyróżnione. Gdy nie pasuje żaden, aplikacja informuje o braku przypisania i pozwala wybrać dowolny plan. Starsze plany domyślnie nie mają przypisanych dni.
- 19 rodzajów sprzętu: 18 piktogramów odtworzonych wektorowo z przesłanej planszy oraz masa ciała. Rozwijany wybór w edytorze ćwiczenia obejmuje m.in. sztangę EZ, maszynę, linę, uchwyty D, drążki tricepsowe, paski i uchwyty do ściągania/wiosłowania. Dotychczasowe nazwy sprzętu zachowano, więc zapisane ćwiczenia otrzymują nowe piktogramy bez utraty wyboru.
- Rzeczywiste ciężary i powtórzenia, zaznaczanie wykonanych serii, poprzedni wynik każdej serii, wznowienie treningu po zamknięciu aplikacji.
- Przerwy czasowe z trwałym terminem zakończenia, automatycznym powrotem do treningu i możliwością ręcznego pominięcia.
- Osobne przerwy między seriami ćwiczenia: w edytorze ćwiczenia wybierz „＋ Odpoczynek po serii”, ustaw czas. Podczas treningu uruchom przerwę przyciskiem pomiędzy seriami. Każda przerwa ma własne ustawienia i można ją usunąć. Odpoczynek jest dostępny wyłącznie między seriami tego samego ćwiczenia.
- Dziennik z kopią pierwotnego planu i rzeczywistymi wynikami. Opcjonalne przeniesienie wykonanych ciężarów i powtórzeń do aktualnego planu.
- Imię i zdjęcie z urządzenia, umowny poziom zmęczenia mięśni, kalendarz, miesięczny wykres objętości osobno dla każdego planu.
- Automatyczny lokalny zapis planów, projektu edycji, treningu, przerwy, historii i profilu w prywatnym magazynie aplikacji (JSON w SharedPreferences).

## Zasady obliczeń

Objętość to suma `kg × powtórzenia` ukończonych serii. Procent zmiany ciężaru serii to `(wykonanie − plan) / plan × 100`; 10 → 15 kg daje +50%. Porównanie objętości odnosi się do całego planu, więc trening niepełny może mieć wynik ujemny. Przy zerowej podstawie pokazujemy kreskę zamiast dzielenia przez zero.

Każda partia z przynajmniej jedną ukończoną serią otrzymuje +2 raz na trening, do limitu 4. Każda kolejna data kalendarzowa zmniejsza poziom o 1. Historyczne treningi są przetwarzane chronologicznie. Poziomy: 0 wypoczęty, 1 żółty, 2 czerwony, 3 fioletowy, 4 szary.

## Zakres wersji 0.1

To funkcjonalna wersja początkowa, bez synchronizacji w chmurze i eksportu danych. Odinstalowanie aplikacji usuwa jej dane. Timer korzysta z czasu urządzenia; w tle nie emituje powiadomień ani alarmów, a po powrocie odtwarza pozostały czas. Gry są celowo testowymi przyciskami zgodnie z promptem.

Przesłany PNG jest planszą referencyjną, a nie eksportem komponentów Figma. Ikony sprzętu są narysowane w Compose, a sylwetka korzysta z dostarczonego PNG; fotografia siłowni została zastąpiona ilustracją sztangi, a typografia korzysta z fontu systemowego. Układ jest inspirowany makietą, nie jest kopią piksel w piksel. Zmiana kolejności segmentów działa przyciskami, bez przeciągania.

## Sprawdzenie

Wersja 0.1.10: nowe logo kowadła jako ikona aplikacji i grafika w nagłówku treningu, powitanie z imieniem po prawej oraz zachowana data. Usunięto nagłówek „Gotowy na mocny trening?” i komunikat o braku planu na dziś. Kolor akcentu #F4762F pochodzi z dostarczonego logo i zastępuje limonkę w przyciskach, nawigacji, wykresach i zaznaczeniach. Semantyczne kolory skali statusu 0–4 pozostają niezależne od koloru marki. Plik logo w `drawable-nodpi/brand_logo.png` jest oryginałem bez modyfikacji.

Wersja 0.1.9 upraszcza profil: nagłówek „STATUS”, sylwetki bez podpisów przód/tył i wyłącznie legenda 0–4 pod spodem. Kolor 0 odpowiada neutralnemu mięśniowi po filtrze ciemnego motywu (#3D443F); 4 jest jaśniejszy, a 1–3 bardziej nasycone. Zasady naliczania i regeneracji pozostają takie same.

Wersja 0.1.8: etykiety mięśni na kafelkach korzystają z dostępnej szerokości i przechodzą do następnego rzędu dopiero, gdy się nie mieszczą. Wszystkie kafelki planów pokazują siedem skrótów dni; bez przypisania wszystkie są przygaszone, a przypisane dni wyróżnia wyłącznie kolor tekstu.

Wersja 0.1.6 upraszcza proponowany kafelek treningu: nagłówek pokazuje wszystkie skróty dni i wyróżnia przypisane, a bez harmonogramu wyświetla „Bez przypisanych dni”. Usunięto powtórzoną listę dni, podpisy przód/tył i etykiety mięśni pod sylwetką wyłącznie na tym kafelku. Pozostałe widoki zachowują podpisy.

Wersja 0.1.5: ciemna kolorystyka sylwetki (filtr podczas renderowania, bez modyfikacji źródłowego PNG), nowe grupy Przedramiona/Kaptury/Szyja z wyborem i regeneracją, sylwetka i etykiety mięśni na głównym kafelku treningu, etykiety na pozostałych planach. Dni tygodnia są w jednym rzędzie, a przycisk wznawiania niezapisanego projektu został usunięty. Ciężar i powtórzenia mają przyciski −/+ zmieniające wartość o 1; pole nadal obsługuje wpisywanie ręczne. Wartości nie schodzą poniżej zera, a limity pozostają 2000 kg i 1000 powtórzeń. Test interfejsu sprawdza oba kierunki zmiany oraz ręczne wpisanie ciężaru dziesiętnego.

Wersja 0.1.4 dodaje harmonogram tygodniowy i oryginalną sylwetkę użytkownika. Testy obejmują wybór planów dla poniedziałku/niedzieli, kilka planów jednego dnia, dni bez treningu, zapis wybranych dni oraz wyświetlanie sylwetki w obu miejscach. Źródłowy PNG znajduje się w `app/src/main/res/drawable-nodpi/muscle_anatomy.png` i pozostaje niezmieniony; interaktywne nakładki tworzy `BodyMap.kt` podczas wyświetlania.

Wersja 0.1.3 aktualizuje piktogramy i rozszerza katalog sprzętu do 19 pozycji. `EquipmentArt.kt` rysuje sylwetki w stałych proporcjach, z kolorem dostosowanym do zaznaczenia. Test interfejsu dodatkowo wybiera nowy długi uchwyt do wiosłowania i sprawdza odczyt tego wyboru z zapisanego projektu planu.

Wersja 0.1.2 usuwa samodzielne segmenty odpoczynku między ćwiczeniami. Przy wczytaniu danych usuwa je również z planów, projektu edycji i aktywnego treningu, zachowując ćwiczenia, wyniki i przerwy między seriami. Trwająca przerwa między ćwiczeniami zostaje zakończona; timer przerwy między seriami pozostaje aktywny. Zapisana historia treningów zachowuje pierwotne dane. Aktualny `PlanEditorTest` sprawdza brak opcji dodania samodzielnej przerwy oraz działanie dodawania ćwiczeń, przerwy między seriami i zapisu planu.

Wersja 0.1.1 usuwa awarię przy dodawaniu ćwiczenia i segmentu odpoczynku: nawigacja otrzymuje dodany segment bez wyszukiwania go w poprzedniej wersji planu. Test interfejsu `PlanEditorTest` sprawdza dodawanie pierwszego i kolejnego ćwiczenia, przerwy między seriami, przerwy między ćwiczeniami oraz zapis planu. Można go uruchomić na osobnym emulatorze przez `gradlew connectedDebugAndroidTest` (test tworzy plan w danych aplikacji testowanego emulatora).

Weryfikacja 0.1.1: 10 testów jednostkowych bez błędów, kompilacja APK i Lint zakończone pomyślnie. Test interfejsu `PlanEditorTest` przeszedł na osobnym emulatorze API 37 (`OK (1 test)`). Biblioteka Espresso 3.7.0 jest przypięta dla zgodności testów z tym systemem. APK można instalować jako aktualizację bez odinstalowywania aplikacji i usuwania danych.

```powershell
$env:JAVA_HOME = 'C:\Program Files\Android\Android Studio\jbr'
.\gradlew.bat testDebugUnitTest assembleDebug lintDebug
```

APK: `app/build/outputs/apk/debug/app-debug.apk`. Testy jednostkowe obejmują procenty, objętość, nieuwzględnianie nieukończonych serii, regenerację w kolejnych dniach i limit zmęczenia.

Ręczny scenariusz: edytuj Góra A → ustaw przerwę i grę → zapisz → rozpocznij trening → wykonaj 15 kg zamiast 10 → ukończ serię → przetestuj przerwę → zakończ trening → sprawdź +50% przy serii → zaktualizuj plan → sprawdź, że dziennik nadal pokazuje pierwotne 10 kg → otwórz profil i kalendarz. Powtórz po ponownym uruchomieniu aplikacji.

Weryfikacja wykonana 23.10.2026: kompilacja APK debug, 7 testów jednostkowych bez błędów i analiza Android Lint (bez błędów, z ostrzeżeniami dotyczącymi m.in. dostępności nowszych zależności). Aplikację zainstalowano i uruchomiono na emulatorze API 37; sprawdzono wizualnie ekran główny. Pełny powyższy scenariusz interaktywny oraz test na fizycznym telefonie pozostają do wykonania.
