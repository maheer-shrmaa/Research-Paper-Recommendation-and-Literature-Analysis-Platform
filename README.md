# Research Paper Recommendation & Literature Analysis Platform

A beginner-friendly full-stack MVP for discovering, recommending, comparing, and analyzing research-paper metadata.

## Stack
HTML5, CSS3, Vanilla JavaScript, Node.js, Express.js, MongoDB, Mongoose, PDFKit.

## Run locally
1. Install Node.js and MongoDB (or use MongoDB Atlas).
2. Copy `.env.example` to `.env` and set `MONGODB_URI`, `JWT_SECRET`, and `OPENAI_API_KEY`. The AI key is used only by the backend and must never be added to frontend files.
3. Run `npm install`.
4. Run `node data/seed.js` to insert demo data.
5. Run `npm start`.
6. Open http://localhost:5000

## Use VS Code Go Live
1. Start the backend with `npm start` in a terminal and leave it running.
2. Open `public/index.html` and select **Go Live**.
3. The workspace Live Server settings serve `public/` at http://localhost:5501, while API requests are sent to the backend at http://localhost:5000.
4. If Live Server was already running, stop it and select **Go Live** again to load the updated workspace settings.

## Accounts
Open `/login.html` to sign in or create an account. Passwords are stored as bcrypt hashes and login sessions use expiring bearer tokens. Set `JWT_SECRET` in `.env` to a long, random value and keep it private; the development fallback changes when the server restarts, which invalidates existing sessions.

## Upload papers
Sign in, open Discover, and upload a PDF up to 10 MB. Uploaded files are stored privately under `data/uploads/`; only the account that uploaded a file can download it. Uploading records its title, optional area/year, and original filename, but does not extract the PDF's text or automatically fill authors, abstract, or keywords.

## Saved papers and library
Saved articles are tied to the signed-in account. Each user keeps a private library, and other users cannot access or modify those saved records.

## Analyze papers
Open Analysis to select one or more records and review their abstracts, recorded methods, datasets, results, stated limitations, and metadata coverage. Record completeness measures how many of five sections are documented; it does not evaluate scientific quality, rigor, or validity. Blank fields and placeholders such as “Not supplied,” “N/A,” and “Not recorded” are treated as undocumented. Selections with no documented sections show an upload/record empty state instead of zero-valued statistics; when only some selected records have documented sections, statistics use documented records only and identify excluded records. Area insights appear only for selections of at least three papers across at least two areas. Demo seed records are labeled and hidden by default; use the Analysis page toggle to include them. Open a paper record and select its **Writing check** tab to review metrics and common grammar/spelling suggestions locally in the browser. Checks are client-side, and suggestions are not a complete grammar review.

The Analysis page also accepts text-based PDFs up to 10 MB for a grounded peer-review assessment. Set `OPENAI_API_KEY` in the backend `.env`; optionally set `OPENAI_MODEL` (defaults to `gpt-4o-mini`) or `OPENAI_BASE_URL` for a compatible endpoint. PDF text is extracted on the server and sent to that configured AI provider; the raw extracted text is not retained. Scores and full review results are saved in MongoDB against the signed-in user. The server validates the structured review, computes the weighted score, and includes the paper's conclusion/limitations when long text must be truncated. Reopen saved analyses from Analysis history and download the report as Markdown or PDF. The feature does not OCR scanned/image-only PDFs and reports a distinct error for PDFs with too little extractable text.

## Important
Seed records are demo metadata for testing and are not verified scholarly records or real citation claims.
