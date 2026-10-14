# GitHub Copilot Instructions for Rendash

When generating code for Rendash:
1. Mobile components must use Expo Router v4, React Native, and NativeWind (Tailwind CSS) matching iOS Dark Mode aesthetics.
2. Background color is `#0B0E14`, card background is `#121824`, primary action color is `#00E599` (Emerald).
3. Backend endpoints must be Next.js App Router route handlers (`export async function POST(req: Request)` etc.) deployed as Vercel Serverless Functions.
4. Database access must go through the shared Prisma client with PostgreSQL connection pooling.
5. Refer to `AGENTS.md` and `docs/SCREENS_ANALYSIS.md` for screen routes and functional specifications.
