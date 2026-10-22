## Surface Product & Proof of Concept (Demo 1)
---
This folder contains critical user journey & audit, proof of concept, and a demo for our credit score assesment web app

### Quick Links
---
- baseline critical user journey: [baseline-cuj.md](baseline-cuj.md)
- system topology: [system-topology.jpg](system-topology.jpg)
- proof of concept: [proof-of-concept.md](proof-of-concept.md)
- live demo link: [borrower live demo](https://evidence-lender.vercel.app/apply) and [lender live demo](https://evidence-lender.vercel.app/review)


### how to setup demo locally

our web app code: [codebase](https://github.com/lukezhang01/evidence-lender)

environment requirements: Requires Node 24 or later

```sh
git clone https://github.com/lukezhang01/evidence-lender.git
cd evidence-lender
npm install
npm run dev
```

Then open:

- Borrower application: http://localhost:3000/apply (upload a CSV from `samples/`)
- Associate review: http://localhost:3000/review