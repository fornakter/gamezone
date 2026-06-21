# Konfiguracja Steam Logowania

## Problem
Przycisk logowania Steam w zakładce logowania nie działa, ponieważ **brak klucza API Steam**.

## Rozwiązanie - Krok po Kroku

### 1. Utwórz klucz API Steam
1. Przejdź na: https://steamcommunity.com/dev/apikey
2. Zaloguj się na swoje konto Steam (jeśli nie jesteś zalogowany)
3. Zaakceptuj warunki umowy
4. W polu "Domain Name" wpisz: `localhost` (dla testów)
5. Kliknij "Zarejestruj"
6. Skopiuj wygenerowany **Steam API Key** (długi ciąg znaków)

### 2. Dodaj klucz do pliku `.env`
Otwórz plik `/home/nakter/Aplikacje/gamezone/xp-website/.env` i zmień:

```
STEAM_API_KEY=your_key_here
```

na:

```
STEAM_API_KEY=paste_your_api_key_here
```

### 3. Uruchom ponownie serwer
Zabij bieżący proces serwera i uruchom go ponownie:

```bash
cd /home/nakter/Aplikacje/gamezone/xp-website
export PATH=/home/linuxbrew/.linuxbrew/opt/node@20/bin:$PATH
npm start
```

### 4. Sprawdź czy Steam jest skonfigurowany
W konsoli powinien pojawić się komunikat:
```
✅ Steam OAuth jest skonfigurowane i gotowe.
```

## Jak działa logowanie Steam?

1. Użytkownik klika przycisk "Steam" w login.html
2. Przekierowanie na `/api/auth/steam`
3. Passport.js otwiera formularz logowania Steam
4. Po zalogowaniu, Steam przekierowuje na `/api/auth/steam/callback`
5. Aplikacja tworzy nowego użytkownika lub loguje istniejącego
6. Sesja jest ustawiana
7. Przekierowanie na stronę główną

## Błędy

### `steam_not_configured`
Oznacza że `STEAM_API_KEY` nie jest ustawiony w `.env`

### `steam_failed`  
Passport.js nie zdołał autoryzować - mogą być problemy z:
- Nieprawidłowym kluczem API
- Problemami z połączeniem do Steam
- Błędami w konfiguracji `STEAM_REALM` lub `STEAM_RETURN_URL`

## Testowanie bez klucza API

Jeśli nie chcesz teraz skonfigurować Steam:
- Użyj **rejestracji email** - działa bez dodatkowej konfiguracji
- Lub poproś administratora o klucz API Steam
