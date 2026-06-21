# GameZone Portal

Serwer portalu GameZone z autentykacją użytkowników i integracją Steam OAuth.

## Features

- 🎮 Steam OAuth 2.0 Integration
- 🔐 Email/Password Authentication
- 💾 SQLite Database
- 🎯 User Profile Management
- 📊 Game Stats Integration

## Prerequisites

- Node.js 20.x
- npm

## Installation

```bash
npm install
```

## Environment Variables

Utwórz plik `.env` w głównym katalogu projektu z następującymi zmiennymi:

```env
# Server
PORT=3000

# Session
SESSION_SECRET=your-session-secret-here-change-in-production

# Steam OAuth 2.0
STEAM_API_KEY=your-steam-api-key-here
STEAM_REALM=http://localhost:3000
STEAM_RETURN_URL=http://localhost:3000/api/auth/steam/callback
```

### Wyjaśnienie zmiennych:

| Zmienna | Opis | Przykład |
|---------|------|---------|
| `PORT` | Port na którym będzie uruchomiony serwer | `3000` |
| `SESSION_SECRET` | Tajny klucz do szyfrowania sesji (zmień w produkcji!) | `your-secret-key` |
| `STEAM_API_KEY` | Klucz API z Steam Developer Portal | `XXXXXXXXXX` |
| `STEAM_REALM` | URL główny aplikacji (dla Steam OAuth) | `https://yourdomain.com` |
| `STEAM_RETURN_URL` | URL callback'u po zalogowaniu Steam | `https://yourdomain.com/api/auth/steam/callback` |

### Pozyskanie Steam API Key:

1. Przejdź na https://steamcommunity.com/dev/apikey
2. Zaloguj się na swoje konto Steam
3. Wpisz domenę aplikacji
4. Skopiuj wygenerowany klucz
5. Dodaj go do zmiennej `STEAM_API_KEY` w `.env`

## Deployment na Render.com

### Konfiguracja zmiennych środowiskowych:

1. W panelu Render.com przejdź do **Environment**
2. Dodaj zmienne:
   - `PORT` → `3000`
   - `SESSION_SECRET` → (wygeneruj bezpieczny klucz)
   - `STEAM_API_KEY` → (twój klucz z Steam)
   - `STEAM_REALM` → `https://your-app.onrender.com`
   - `STEAM_RETURN_URL` → `https://your-app.onrender.com/api/auth/steam/callback`

3. W ustawieniach Steam Developer Portal zaktualizuj:
   - **App Domain** → `your-app.onrender.com`
   - **Login Redirect URL** → `https://your-app.onrender.com/api/auth/steam/callback`

## Development

```bash
npm start
```

Serwer będzie dostępny pod adresem `http://localhost:3000`

## Project Structure

```
xp-website/
├── database/          # Inicjalizacja bazy danych
├── middleware/        # Express middleware
├── public/           # Pliki statyczne (HTML, CSS, JS)
├── routes/           # API endpoints
│   ├── auth.js       # Autentykacja
│   └── steam.js      # Steam OAuth
├── server.js         # Główny plik serwera
├── .env              # Zmienne środowiskowe (nie wersjonuj!)
└── package.json      # Zależności
```

## Endpoints

### Autentykacja
- `POST /api/auth/register` - Rejestracja email/hasło
- `POST /api/auth/login` - Logowanie email/hasło
- `POST /api/auth/logout` - Wylogowanie
- `GET /api/auth/me` - Dane aktualnego użytkownika
- `GET /api/auth/steam` - Inicjacja Steam OAuth

### Steam
- `GET /api/steam/games` - Lista gier użytkownika
- `GET /api/steam/profile` - Profil Steam użytkownika

## Security Notes

⚠️ **Ważne:**
- Nigdy nie commituj pliku `.env` do Git
- Zmień `SESSION_SECRET` w produkcji na bezpieczny klucz
- Używaj HTTPS w produkcji
- Przechowuj Steam API Key bezpiecznie

## License

MIT
