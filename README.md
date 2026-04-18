# Regression Dashboard

A **Windows Metro-style live tiles** dashboard for monitoring regression and CI/CD process statuses in real time.

![Dashboard Preview](https://img.shields.io/badge/style-Windows%20Metro-0078D4?style=flat-square&logo=windows)
![Vanilla JS](https://img.shields.io/badge/vanilla-JS-FFB900?style=flat-square&logo=javascript)
![No Build Required](https://img.shields.io/badge/build-none%20required-107C10?style=flat-square)

---

## Features

- 🟦 **Windows Metro tile grid** — tiles of varying sizes (1×1, 2×1, 1×2, 2×2)
- 🎨 **Status colour coding** — Running, Passed, Failed, Pending, Stopped, Warning
- ⚡ **Animated tile transitions** — flip & flash when a status changes
- 📊 **Summary bar** — live count of each status at the top
- 🔄 **Auto-refresh** — configurable interval (default 5 s) with countdown display
- ⏸ **Pause / Resume** — freeze updates without closing the page
- 🌙 **Dark / Light theme** — toggle in the header
- 📱 **Responsive** — works on desktop, tablet, and mobile

---

## Quick Start

1. Clone or download the repository.
2. Open `index.html` in any modern browser — no build step required.

```bash
git clone https://github.com/iDV-asic/RegressionDashboard.git
cd RegressionDashboard
# open index.html in your browser
```

---

## File Structure

```
RegressionDashboard/
├── index.html      ← Main dashboard page
├── styles.css      ← Metro tile styles & animations
├── dashboard.js    ← Process data, status logic, auto-refresh
└── README.md
```

---

## Customisation

### Add / remove processes

Edit the `PROCESSES` array in `dashboard.js`:

```js
const PROCESSES = [
  { id: 'my_job', name: 'My Job', size: '2x1', status: 'pending', progress: 0, live: true },
  // ...
];
```

| Field      | Type              | Description                                       |
|------------|-------------------|---------------------------------------------------|
| `id`       | string            | Unique key (used for DOM IDs)                     |
| `name`     | string            | Display name on the tile                          |
| `size`     | `1x1` / `2x1` / `1x2` / `2x2` | Tile dimensions               |
| `status`   | see below         | Initial status                                    |
| `progress` | 0–100 or `null`   | Shown as a bar for `running`/`pending` tiles      |
| `live`     | boolean           | Show the **LIVE** badge                           |

### Statuses

| Key       | Colour  |
|-----------|---------|
| `running` | Blue    |
| `passed`  | Green   |
| `failed`  | Red     |
| `pending` | Amber   |
| `warning` | Orange  |
| `stopped` | Grey    |

### Change refresh interval

```js
// dashboard.js — top of file
const REFRESH_INTERVAL_MS = 5000; // milliseconds
```

### Connect to a real backend

Replace `simulateUpdate()` in `dashboard.js` with a `fetch()` call to your API:

```js
async function simulateUpdate() {
  const data = await fetch('/api/status').then(r => r.json());
  data.forEach(proc => {
    const local = processes.find(p => p.id === proc.id);
    if (local) {
      const oldStatus = local.status;
      Object.assign(local, proc);
      updateTile(local, oldStatus !== local.status);
    }
  });
  renderSummary();
}
```

---

## License

MIT