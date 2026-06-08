# 🏠 Home Assistant — Konfiguracje, automatyzacje i blueprinty

To moje prywatne repozytorium konfiguracji [Home Assistant](https://www.home-assistant.io/).
Zawiera automatyzacje YAML oraz **blueprinty** automatyzacji udostępnione publicznie
do importowania w innych instancjach HA (np. przez
[blueprints-updater](https://github.com/luuquangvu/blueprints-updater)).

---

## 📦 Struktura repozytorium

```
homeassistant/
├── automations/                       # 🤖 Lokalne automatyzacje (mojej instancji)
│   ├── auto_calibrate_thermostat.yaml
│   └── auto_set_temperature.yaml
│
├── blueprints/                        # 📐 Blueprinty (możliwe do importu)
│   └── automation/
│       └── szwajowy/
│           └── ikea_dimmer_light_control.yaml
│
├── LICENSE                            # 📜 Licencja MIT
└── README.md
```

---

## 📐 Blueprinty

### 🔘 IKEA Dimmer — Sterowanie oświetleniem
**Plik:** [`blueprints/automation/szwajowy/ikea_dimmer_light_control.yaml`](blueprints/automation/szwajowy/ikea_dimmer_light_control.yaml)

Uniwersalny blueprint do sterowania oświetleniem pilotami **IKEA RODRET Dimmer (E2201)**
oraz **IKEA TRADFRI on/off switch (E1743)** w obu integracjach Zigbee
(**ZHA** oraz **Zigbee2MQTT**).

#### ✨ Funkcjonalność
- 💡 **Płynne ściemnianie** — przytrzymaj przycisk, jasność rośnie/maleje aż do puszczenia
- 👆 **Wykrywanie multi-tap** — single / double / triple tap z konfigurowalnym oknem czasowym
- 🎯 **Dowolne akcje** — każdy slot przycisku akceptuje dowolną sekwencję akcji HA
  (sceny, kolory, skrypty, dowolne service calls)
- 🌙 **Tryb nocny** — automatyczna zmiana jasności i temperatury barwowej wg `schedule.*`
- 🎨 **Domyślne ustawienia włączenia** — opcjonalne nadpisanie zapamiętanej jasności / koloru
- 🚫 **Globalna blokada** — opcjonalny warunek blokujący wszystkie akcje
- 📝 **Logbook** — opcjonalne logowanie akcji do dziennika zdarzeń
- 🎛️ **Cel sterowania** — pojedyncze światło, grupa lub cały pokój (selector `target`)

#### 🛠️ Konfiguracja
Wszystkie ustawienia pogrupowane w 7 zakładek:
1. **📱 Urządzenie i oświetlenie** — wybór pilota + integracji + świateł
2. **🟢 Przycisk WŁĄCZ** — akcje dla każdego rodzaju naciśnięcia
3. **🔴 Przycisk WYŁĄCZ** — akcje dla każdego rodzaju naciśnięcia
4. **💡 Jasność i ściemnianie** — krok, prędkość, min/max, zachowanie skrajnych wartości
5. **🌙 Tryb nocny** — harmonogram + jasność nocna + temp. barwowa
6. **🎨 Domyślne włączenie** — jasność/kolor/temp. przy domyślnym włączeniu
7. **🔧 Zaawansowane** — multi-tap, blokada, logowanie, smooth transition

#### 📥 Import do Home Assistanta
1. **Otwórz HA** → Ustawienia → Automatyzacje i sceny → **Blueprinty**
2. Kliknij **„Importuj blueprint"** w prawym dolnym rogu
3. Wklej URL:
   ```
   https://github.com/Szwajowy/homeassistant/blob/master/blueprints/automation/szwajowy/ikea_dimmer_light_control.yaml
   ```
4. Kliknij **„Załaduj blueprint"**, potwierdź nazwę i zapisz
5. Stwórz automatyzację z blueprintu: Ustawienia → Automatyzacje → **Stwórz automatyzację** → wybierz blueprint

#### 🔄 Automatyczna aktualizacja
Korzystam z addonu [blueprints-updater](https://github.com/luuquangvu/blueprints-updater)
do automatycznego sprawdzania nowych wersji blueprintów. Pole `source_url` w blueprintcie
wskazuje na ten plik, więc updater pobierze najnowszą wersję bez ręcznej interwencji.

#### 🧪 Wsparcie i debugowanie
- ⚠️ **Z2MQTT**: pozostaw pole "Pilot IKEA" puste i wpisz pełny topic MQTT (np. `zigbee2mqtt/pilot_salon`)
- ⚠️ **ZHA**: pozostaw pole "Topic MQTT" puste i wybierz pilot z listy urządzeń
- 📝 W przypadku problemów włącz **„Loguj zdarzenia w dzienniku"** w sekcji `🔧 Zaawansowane`
  — każde naciśnięcie pilota zostanie zalogowane wraz z informacją o trybie nocnym i multi-tap

#### 🗺️ Mapowanie zdarzeń pilota
| Akcja           | ZHA `type` / `subtype`                                | Z2MQTT `action`         |
|-----------------|-------------------------------------------------------|-------------------------|
| Klik WŁĄCZ      | `remote_button_short_press` / `turn_on`               | `on`                    |
| Klik WYŁĄCZ     | `remote_button_short_press` / `turn_off`              | `off`                   |
| Hold WŁĄCZ      | `remote_button_long_press` / `dim_up`                 | `brightness_move_up`    |
| Hold WYŁĄCZ     | `remote_button_long_press` / `dim_down`               | `brightness_move_down`  |
| Puszczenie WŁĄCZ| `remote_button_long_release` / `dim_up`               | `brightness_stop`       |
| Puszczenie WYŁĄCZ| `remote_button_long_release` / `dim_down`            | `brightness_stop`       |

⚠️ **Double/triple tap** nie są wspierane natywnie przez te piloty — blueprint wykrywa je
samodzielnie poprzez okno czasowe (`multi_tap_window`, default 400 ms).
Włącz `enable_double_tap` / `enable_triple_tap` w sekcji `🔧 Zaawansowane`, aby je aktywować.
⚠️ Włączenie multi-tap powoduje **opóźnienie pojedynczego klika** o czas okna wykrywania.

#### 📌 Wymagania
- Home Assistant **2024.10.0** lub nowszy
- Integracja **ZHA** *lub* **Zigbee2MQTT** + broker **MQTT**
- Co najmniej jeden pilot **IKEA RODRET (E2201)** lub **IKEA TRADFRI (E1743)** sparowany

---

## 🤖 Lokalne automatyzacje

Folder [`automations/`](automations/) zawiera moje prywatne automatyzacje używane w domowej instancji.
Nie są one przeznaczone do bezpośredniego użycia w innych instancjach — bazują na konkretnych
encjach z mojego setupu. Mogą natomiast posłużyć jako inspiracja.

| Plik | Opis |
|------|------|
| [`auto_calibrate_thermostat.yaml`](automations/auto_calibrate_thermostat.yaml) | Automatyczna kalibracja termostatów TRV na podstawie zewnętrznych czujników temperatury |
| [`auto_set_temperature.yaml`](automations/auto_set_temperature.yaml) | Ustawianie temperatury w pokojach wg harmonogramu i obecności |

---

## 📜 Licencja

Projekt udostępniony na licencji **MIT** — pełny tekst w pliku [`LICENSE`](LICENSE).

W skrócie: możesz **swobodnie używać, modyfikować i udostępniać** ten kod (komercyjnie też),
pod warunkiem zachowania w kopii oryginalnej notki o prawach autorskich. Kod dostarczany "as-is"
— bez gwarancji.

---

## 🤝 Wkład

Jeśli znalazłeś/aś błąd lub masz propozycję usprawnienia blueprintu — zachęcam do otwarcia
[issue](https://github.com/Szwajowy/homeassistant/issues) lub
[pull request](https://github.com/Szwajowy/homeassistant/pulls). 🙏
