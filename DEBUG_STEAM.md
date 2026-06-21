# Debugowanie - Logowanie Steam GameZone

## Znalezione problemy i rozwiązania

### Problem 1: Steam API Key nie jest skonfigurowany ✓ ROZWIĄZANE

**Symptom:** Klikanie przycisku Steam na login.html powoduje przekierowanie na 
`/login.html?error=steam_not_configured`

**Przyczyna:** Plik `.env` nie zawiera `STEAM_API_KEY`

**Rozwiązanie:**
1. Idź do: https://steamcommunity.com/dev/apikey
2. Zaloguj się na Steam
3. Wpisz `localhost` w Domain Name (dla testów lokalnych)
4. Skopiuj wygenerowany klucz
5. Otwórz `.env` w projekcie i wstaw:
   ```
   STEAM_API_KEY=YOUR_COPIED_KEY_HERE
   ```
6. Uruchom ponownie serwer

---

### Problem 2: Zły URL callback w środowisku produkcyjnym

**Przyczyna:** `STEAM_RETURN_URL` w `.env` musi być dostępny z internetu

**Rozwiązanie dla produkcji:**
```env
STEAM_RETURN_URL=https://your-domain.com/api/auth/steam/callback
STEAM_REALM=https://your-domain.com/
```

---

### Problem 3: Konfiguracja sesji

**Aktualne ustawienie w server.js:**
- Sesje przechowywane w **pamięci** (ulotne - znikają przy restarcie serwera)
- Cookies są ustawione prawidłowo

**Dla produkcji - dodaj store sesji (MongoDB, Redis, etc.)**

---

## Testy które możesz wykonać

### Test 1: Sprawdzanie czy .env jest przeczytany
```bash
cd /home/nakter/Aplikacje/gamezone/xp-website
export PATH=/home/linuxbrew/.linuxbrew/opt/node@20/bin:$PATH
npm start
```

Poszukaj w konsoli:
- `✅ Steam OAuth jest skonfigurowane i gotowe.` - Steam działa
- `ℹ️  INFO: Logowanie przez Steam nie jest skonfigurowane.` - Klucz brakuje

### Test 2: Sprawdzenie endpointu Steam
```bash
curl http://localhost:3000/api/auth/steam
```

Powinieneś zobaczyć:
- Jeśli Steam jest skonfigurowany: prośba o zalogowanie się w Steam
- Jeśli nie: redirect na `/login.html?error=steam_not_configured`

### Test 3: Sprawdzenie sesji użytkownika
Po zalogowaniu przejdź na:
```
GET http://localhost:3000/api/auth/me
```

Powinna zwrócić:
```json
{
  "id": 1,
  "username": "steam_username",
  "email": "steam_12345@gamezone.local",
  "steamId": "12345...",
  "steamUsername": "steam_username",
  "avatarUrl": "https://..."
}
```

---

## Plik .env - Prawidłowa konfiguracja

```env
PORT=3000
SESSION_SECRET=your-random-secret-key
STEAM_API_KEY=YOUR_STEAM_API_KEY_HERE
STEAM_REALM=http://localhost:3000
STEAM_RETURN_URL=http://localhost:3000/api/auth/steam/callback
```

---

## Ogólne porady

1. **Sprawdzaj konsolę przeglądarki** - F12 → Console tab (błędy JavaScript)
2. **Sprawdzaj konsolę serwera** - terminal gdzie uruchamiasz `npm start`
3. **Czyszczenie cache** - Ctrl+Shift+Delete w przeglądarce
4. **Cookies** - F12 → Application → Cookies (sprawdzaj czy sesja jest zapisywana)

---

## Status konfiguracji

| Element | Status | Notatka |
|---------|--------|---------|
| Node.js 20 | ✅ | Zainstalowany i działa |
| Express server | ✅ | Uruchomiony na :3000 |
| Passport | ✅ | Skonfigurowany |
| .env file | ✅ | Istnieje i jest czytany |
| STEAM_API_KEY | ❌ | **WYMAGANE** - brakuje klucza |
| Email login | ✅ | Działa bez klucza Steam |
| Steam login | ⏳ | Czeka na STEAM_API_KEY |
