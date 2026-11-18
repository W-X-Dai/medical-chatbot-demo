# Medical chatbot demo (static build)

Production build of the ELVINA pre-consultation web frontend (Vue 3 + Vite),
frontend version 0.19.2, built for the base path `/medical-chatbot-demo/` so it
loads on GitHub Pages at https://w-x-dai.github.io/medical-chatbot-demo/.

This is a static bundle only. Consultations need the FastAPI backend reachable
from the same origin at `/api` (the build uses `VITE_BACKEND_BASE_URL=/api`);
without it the pages load but cannot start an interview. GitHub Pages has no
backend, so there the pages render but consultations, the doctor workspace and
the Rules Center report that the backend is unreachable.

## Contents

- `index.html`, `launch.html`, `assets/` — the built application
- `avatar-clips/` — pre-rendered AI doctor clips (CosyVoice3 speech, MuseTalk
  lip-sync) played directly by the patient page, plus `manifest.json`
- `dr training pc.png` — the AI-generated doctor image used by the avatar
- `script_review.html` — standalone offline form for clinicians to review the 30
  synthetic validation scenarios; answers stay in the reviewer's browser until
  exported as JSON (no network requests)

## Serving

To run consultations, serve this folder at `/medical-chatbot-demo/` and proxy
`/api/` to the backend on the same origin, for example with nginx:

```nginx
location /medical-chatbot-demo/ { alias /srv/medical-chatbot-demo/; try_files $uri $uri/ /medical-chatbot-demo/index.html; }
location /api/ { proxy_pass http://127.0.0.1:18000/; }
```

All patients in the demo are synthetic. The avatar and its voice are synthetic
and do not represent a real clinician; the system is not a medical device and
its output is not a diagnosis.
