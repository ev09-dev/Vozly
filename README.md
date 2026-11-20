# Vozly

## TikTok OAuth (official Login Kit)

The UI starts OAuth at `/auth/tiktok`; the Node server redirects to TikTok's official authorization URL, uses a one-time random state to defend against CSRF, handles the callback, exchanges the authorization code server-side at `open.tiktokapis.com/v2/oauth/token/`, and retrieves the basic profile from the official user info endpoint. The client secret and access/refresh tokens are never sent to browser JavaScript.

1. Register a web app in [TikTok for Developers](https://developers.tiktok.com/) and enable Login Kit with the `user.info.basic` scope.
2. Add the exact redirect URL `http://localhost:4173/auth/tiktok/callback` to the TikTok app's allowed redirect URLs (or set another `TIKTOK_REDIRECT_URI`).
3. Copy `.env.example` to `.env` and provide `TIKTOK_CLIENT_KEY`, `TIKTOK_CLIENT_SECRET`, and the matching redirect URI. Export these values in your shell (or load them with your preferred environment manager; Node has no dotenv dependency).
4. Run `node server.js` from this directory and open `http://localhost:4173`.

OAuth cannot complete until TikTok has issued credentials and approved the app/scopes. This lightweight demo server keeps tokens in memory; restarting it requires reconnecting. For production, use HTTPS, user-bound encrypted token storage, refresh-token rotation, authenticated API endpoints, and validate ownership/authorization for every account operation. The OAuth connection itself is real; fetching TikTok comment data requires separate API access/permissions and is not claimed here.
