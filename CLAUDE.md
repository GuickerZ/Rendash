# Claude Code Instructions - Rendash

This repository is **Rendash**, a premium financial intelligence app built with:
- **Mobile:** React Native, Expo SDK 52 (Expo Router v4), NativeWind (Tailwind CSS), iOS HIG Dark Mode.
- **Backend:** Next.js (App Router, Turbopack, Serverless Functions on Vercel).
- **Database:** PostgreSQL (Vercel Postgres / Neon) with Prisma ORM.

## Essential Reference Files
- Master Agent Instructions: `AGENTS.md` and `.agents/SYSTEM_PROMPT.md`
- Complete 46 Screens Specification: `docs/SCREENS_ANALYSIS.md`
- System Architecture: `docs/ARCHITECTURE.md`
- Database Schema: `docs/DATABASE_SCHEMA.prisma`
- Serverless API Routes: `docs/API_ROUTES.md`
- Setup & Deploy Guide: `docs/SETUP_GUIDE.md`
- Specialized Skills: `.agents/skills/`

## Coding Standards
- Strictly enforce TypeScript types.
- Follow iOS Dark Theme design system tokens (`#0B0E14` background, `#121824` cards, `#00E599` emerald accents).
- Implement Haptic Feedback on interactive mobile components.
- Wrap financial transfers, investments, and redemptions in `prisma.$transaction`.
