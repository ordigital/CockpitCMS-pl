# CockpitCMS-pl
Polskie tłumaczenie dla CockpitCMS.
https://getcockpit.com/

## Instalacja
- pobierz pliki:
```bash
$ wget https://github.com/ordigital/CockpitCMS-pl/archive/master.zip
```
- rozpakuj je do głownego katalogu CockpitCMS tak, aby plik `pl.php` znalazł się w katalogu `/config/cockpit/i18n/`
- Dodaj język polski w `/config/config.yaml`:
```yaml
i18n: 'pl'
languages:
  pl: 'Polski'
  en: 'English'
```
- Zmień język w edytując ustawienia użytkownika

## Możliwe problemy
Jeśli nie wszystkie elementy panelu są przetłumaczone, sprawdź czy po zalogowaniu działa ścieżka `/cockpit.i18n.data`. Jeśli nie to wykonaj poniższe operacje:
- w pliku `/modules/Cockpit/admin.php` zmień ścieżkę dla tłumaczenie dla elementów panelu w js:
```diff
   if ($translationspath = $this->path("#config:cockpit/i18n/{$locale}.php")) {
        $this->helper('i18n')->locale = $locale;
        $this->helper('i18n')->load($translationspath, $locale);
    }

-   $this->bind('/cockpit.i18n.data', function() {
+   $this->bind('/i18n', function() {
        $this->response->mime = 'js';
        $data = $this->helper('i18n')->data($this->helper('i18n')->locale);
        return 'if (i18n) { i18n.register('.(count($data) ? json_encode($data):'{}').'); }';
    });
```
- tę samą ścieżkę podaj w pliku `/modules/Cockpit/views/layouts/app.php`:
```diff
    {{ $app->assets($app('admin')->data->get('assets'), $app['debug'] ? time() : $app['cockpit/version']) }}

-   <script src="@route('/cockpit.i18n.data')"></script>
+   <script src="@route('/i18n')"></script>

    <script>
        App.$data = {{ json_encode($app('admin')->data->get('extract')) }};
        UIkit.modal.labels.Ok = App.i18n.get(UIkit.modal.labels.Ok);
        UIkit.modal.labels.Cancel = App.i18n.get(UIkit.modal.labels.Cancel);
    </script>
```
