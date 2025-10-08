## AI Resume Analyzer

A small React + TypeScript app that analyzes and scores resumes for ATS (Applicant Tracking System) compatibility and provides a visual score and summary. It uses PDF processing to render pages, a simple scoring UI, and a few components for visualization.

This repository contains the frontend application built with React (v19), Vite, and TypeScript. It is structured to run in development with a dev server and can be built for production. A multi-stage Dockerfile is included for containerized builds.

## Features

- Upload PDF resumes and render pages as images using pdfjs.
- Basic ATS scoring and visualizations (score badges, gauges, summary views).
- Drag-and-drop file upload via `react-dropzone`.
- Lightweight state with `zustand` and utility components for details and review.

## Requirements

- Node.js 20+ (the Dockerfile uses node:20-alpine)
- npm (the project uses npm scripts shown below)

## Quick start

Install dependencies:

```bash
npm ci
```

Run the development server:

```bash
npm run dev
```

Build for production:

```bash
npm run build
```

Serve the built app (the project includes a small server via react-router):

```bash
npm run start
```

Type checking / generate type info:

```bash
npm run typecheck
```

## Docker

This repo includes a multi-stage Dockerfile that installs dependencies, builds the app, and produces a small production image.

Build the image (run from repository root):

```bash
docker build -t ai-resume-analyzer:latest .
```

Run the container:

```bash
docker run --rm -p 3000:3000 ai-resume-analyzer:latest
```

Note: The container runs the `npm run start` command exposed in the Dockerfile. Adjust port mapping if you change the server configuration.

## Project structure (important files)

- `app/` - main React app source
  - `components/` - UI components (Accordion, ATS, FileUploader, ScoreBadge, ScoreGauge, etc.)
  - `routes/` - top-level route components (upload, resume view, auth, wipe)
  - `lib/` - helper utilities (PDF to image conversion, utils)
- `public/` - static assets (icons, images, pdf.worker)
- `package.json` - npm scripts and dependencies
- `Dockerfile` - multi-stage build for containerized deployment
- `tsconfig.json` - TypeScript configuration

## How it works (high level)

1. The user uploads a resume PDF via the `FileUploader` component.
2. The app uses utilities in `app/lib` (and `pdfjs-dist`) to render PDF pages into images for display.
3. The UI components evaluate the content for basic ATS-friendly heuristics and display a score and visualizations.
4. The score components (`ScoreBadge`, `ScoreGauge`, `ScoreCircle`) present quick feedback and the `Summary` component outlines suggestions.

## Development notes

- The app uses TypeScript and Tailwind CSS. Tailwind is configured as a dev dependency.
- Routes and server helpers use `@react-router/*` packages — the build produces a server bundle which is started with `npm run start`.
- PDF processing relies on `pdfjs-dist` and includes a `pdf.worker.min.mjs` in `public/` for faster client-side rendering.

## Contributing

Contributions are welcome. Please open issues for bugs or feature requests and submit pull requests for fixes.

Suggested workflow:

```bash
git checkout -b feature/your-feature
# make changes
npm ci
npm run dev
```

## Troubleshooting

- If PDF pages don't render, ensure `public/pdf.worker.min.mjs` is present and accessible.
- If type errors appear, run `npm run typecheck` to see TypeScript diagnostics.

## License

No license file is included in this repository. Add a `LICENSE` file if you wish to specify one (for example, MIT).

## Acknowledgements

- Built with React, Vite, React Router, pdfjs-dist, and small open-source utilities.

---

If you'd like, I can also add a short CONTRIBUTING.md, a LICENSE, or small usage screenshots taken from `public/images` to the README. Tell me which you'd prefer next.
