# Assessment Tool — Frontend

A React-based web application for administering, scoring, and generating psychological assessment reports for children. Built during an internal hackathon at **[Neuro-Paradigm](https://github.com/neuro-paradigm)**, a research-based startup.

---

## Related Repositories

| Repository | Description |
|---|---|
| **This repo** — [Assessment-Tool-frontend](https://github.com/ChitimellaLaasya/Assessment-Tool-frontend) | React frontend (this project) |
| [Assessment-Tool-backend](https://github.com/ChitimellaLaasya/Assessment-Tool-backend) | Backend API for TQ scoring and PDF report generation |

---

## Overview

This tool streamlines the process of conducting and documenting **Wechsler Intelligence Scale for Children (WISC)** assessments. A clinician enters patient details, raw test scores, and recommendations through a guided multi-step form. The app communicates with a backend API to compute **TQ (Test Quotient) scores** and ultimately generates a downloadable PDF assessment report.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | React 19 |
| Build Tool | Vite 6 |
| Styling | Tailwind CSS v4 |
| Routing | React Router DOM v7 |
| Form Management | React Hook Form v7 |
| Selects | React Select v5 |
| ID Generation | UUID v11 |
| Linting | ESLint 9 |

---

## Project Structure

```
report-frontend/
├── public/
│   └── vite.svg
├── src/
│   ├── assets/
│   │   └── norm_tables/        # Reference norm table images (ages 6–15)
│   │       ├── 6.jpg
│   │       ├── 7.jpg
│   │       └── ...15.jpg
│   ├── components/
│   │   ├── AddPatientForm.jsx       # Root form component with tab navigation
│   │   ├── PersonalDetailsTab.jsx   # Tab 1: Patient demographics
│   │   ├── TestInformationTab.jsx   # Tab 2: Test metadata
│   │   ├── VerbalTestsTab.jsx       # Tab 3: Verbal subtest scores & TQ calculation
│   │   ├── PerformanceTestsTab.jsx  # Tab 4: Performance subtest scores & TQ calculation
│   │   ├── RecommendationsTab.jsx   # Tab 5: Summary, recommendations & report generation
│   │   └── NormTableModal.jsx       # Overlay modal for viewing norm tables
│   ├── App.css
│   ├── App.jsx                      # App root with React Router setup
│   └── main.jsx
├── index.html
├── package.json
├── eslint.config.js
└── vite.config.js
```

---

## Features

### Multi-Step Guided Form
The form is split into **5 sequential tabs** with a visual progress stepper:

1. **Personal Details** — Patient name, gender, date of birth, date of testing, class/grade, informant, and school. Age is auto-calculated from DOB and test date.
2. **Test Information** — Type of test administered (e.g., WISC), reading age, and spelling age.
3. **Verbal Tests** — Raw scores for six verbal subtests: Information, Comprehension, Arithmetic, Similarities, and either Vocabulary **or** Digit Span (mutually exclusive). Calculates TQ scores and Verbal IQ via the backend API.
4. **Performance Tests** — Raw scores for five performance subtests: Picture Completion, Block Design, Object Assembly, Coding, and Mazes. Calculates Performance TQ scores and Full-Scale IQ via the backend API.
5. **Recommendations** — Clinical summary text, dynamic recommendation fields, and buttons to **Preview** or **Download** the final assessment report as a PDF.

### TQ Score Calculation
On the Verbal and Performance tabs, clicking "Calculate" sends raw scores and patient age to the backend `/api/getAllTQScores` endpoint. The UI displays individual subtest TQ scores, the total raw score, the total TQ score, and the average IQ.

### Norm Table Reference Modal
A **"Show Norm Tables"** button opens a modal displaying reference norm table images for ages 6–15. The modal supports navigation between age tables, zoom in/out, and drag-to-pan on zoomed images.

### Report Generation
The Recommendations tab sends all collected form data (personal details, test results, and recommendations) to the backend. The backend generates and returns a formatted PDF report, which is automatically downloaded as `Assessment_Report.pdf`. A preview mode is also available.

### UX Details
- **Enter key navigation**: Pressing Enter advances focus to the next input field within each tab.
- **Per-tab validation**: Each tab validates its own fields before allowing the user to proceed to the next tab.
- **Intermediate state persistence**: Scores like `childAge` and `OverallTQ` are temporarily stored in `localStorage` for cross-tab access.

---

## Backend API

This frontend depends on a separate backend. The following endpoints are used:

| Endpoint | Method | Description |
|---|---|---|
| `/api/getAllTQScores` | POST | Accepts raw subtest scores + age, returns TQ scores and IQ |
| `/download-preview-pdf` | POST | Accepts all form data, returns assessment PDF as binary |
| `/generate-preview` | POST | Accepts all form data, returns HTML preview of the report |

> The backend is deployed separately. See the [Assessment-Tool-backend](https://github.com/ChitimellaLaasya/Assessment-Tool-backend) repository for setup instructions.
>
> Default backend URL used in this project: `https://reports-generation-private-1.onrender.com`

---

## Getting Started

### Prerequisites

- Node.js >= 18
- npm >= 9

### Installation

```bash
git clone https://github.com/ChitimellaLaasya/Assessment-Tool-frontend.git
cd Assessment-Tool-frontend
npm install
```

### Running Locally

```bash
npm run dev
```

The app will be available at `http://localhost:5173`.

### Building for Production

```bash
npm run build
```

The output will be in the `dist/` directory.

### Preview Production Build

```bash
npm run preview
```

---

## Environment / Configuration

The backend API base URL is currently hardcoded in the component files. To make it configurable, create a `.env` file at the project root:

```env
VITE_API_BASE_URL=https://your-backend-url.onrender.com
```

Then replace the hardcoded URLs in the components with `import.meta.env.VITE_API_BASE_URL`.

---

## Notes

- This project was built during an internal hackathon at [Neuro-Paradigm](https://github.com/neuro-paradigm), a research-based startup.
- The frontend and backend are maintained as separate repositories.
- Some components contain commented-out legacy code from earlier iterations of the form.
- Norm table images (ages 6–15) are bundled as static assets in `src/assets/norm_tables/`.
