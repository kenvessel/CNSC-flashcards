# NutriDx — CNSC Flashcard Review

A phone-friendly Progressive Web App version of the NutriDx CNSC flashcard tool.

## What this version does
- Keeps the existing 636 flashcards and study interface.
- Saves flashcard progress, flags, scheduling data, and streak in the browser's local storage.
- Can be installed to a phone home screen as a standalone app.
- Includes a service worker so the app can continue working offline after it has been loaded once.

## GitHub Pages
1. Create a GitHub repository (recommended name: `CNSC-Flashcards`) and keep it **Private** if you do not want the flashcard content public.
2. Upload all files in this folder to the repository root.
3. In GitHub: Settings → Pages → Deploy from a branch → choose the main branch and `/ (root)` → Save.
4. Open the resulting Pages URL on your phone.
5. On iPhone Safari: Share → Add to Home Screen.

## Important storage note
This version uses browser local storage, so progress persists on the same device/browser but is not automatically synchronized to other devices. A later version can add cloud sync (for example with Supabase) and account-based backup.
