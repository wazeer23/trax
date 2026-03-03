# Sports Finder

A mobile-first Flutter application for discovering nearby sports activities, courts, and facilities. Built as a full-stack project demonstrating real-time geospatial data, AI-enhanced content, and cross-platform deployment.

**[Live Demo](https://sportsfinderapp.vercel.app)**

---

## Overview

Sports Finder helps users find basketball courts, tennis courts, pickleball facilities, soccer fields, playgrounds, and 15+ other activity types near any location. The app combines multiple data sources with AI enhancement to provide accurate, detailed facility information.

## Key Features

- **Interactive Map** with custom-styled tiles, marker clustering, and animated transitions
- **15+ Activity Types** including basketball, tennis, pickleball, soccer, volleyball, golf, swimming, biking, fishing, kayaking, and more
- **Smart Filtering** with activity-specific filter chips and real-time map updates
- **Activity Detail Pages** with weather integration, facility details, and navigation
- **Address Search** powered by OpenStreetMap Nominatim (free geocoding)
- **Multiple Map Styles** including City View, Adventure, and Illustrated themes
- **Dark Mode** with automatic theme detection
- **AI-Enhanced Descriptions** using Gemini for facility detail extraction
- **Custom Animations** including bounce effects, page transitions, and animated markers

## Architecture

### Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | Flutter (Dart) — cross-platform iOS, Android, Web |
| **State Management** | Provider pattern with ChangeNotifier |
| **Map Rendering** | Google Maps Flutter (Android/Web), Apple Maps (iOS) |
| **Database** | Supabase (PostGIS) for geospatial activity storage |
| **Geocoding** | OpenStreetMap Nominatim (free) |
| **AI Enhancement** | Google Gemini 1.5 Flash for facility descriptions |
| **Web Scraping** | Crawl4AI for park district data extraction |
| **Deployment** | Vercel (web), Xcode (iOS) |

### Data Pipeline

```
Park District Websites ──> Crawl4AI ──> Gemini AI Analysis ──> Supabase DB
OpenStreetMap Data ─────────────────────────────────────────>
Google Maps (tiles only) ───────────────────────────────────> Flutter App
```

- **Activity data** is sourced from OpenStreetMap, park district websites, and satellite analysis
- **AI enhancement** uses Gemini 1.5 Flash at ~$0.0003/park for facility detail extraction
- **Geocoding** uses free OpenStreetMap Nominatim — no paid geocoding APIs
- **Map tiles** are the only paid API (Google Maps free tier covers ~28,500 loads/month)

### Project Structure

```
lib/
  models/        # Data classes (Activity, Court, Event)
  providers/     # State management (Activities, Location, Filter, Theme)
  screens/       # Full-page UI (Map, Home, Detail, Events, Profile)
  services/      # Business logic (Supabase, Weather, Geocoding, AI)
  widgets/       # Reusable components (Filters, Popups, Nav, Search)
  utils/         # Helpers (Map styles, Icons, Clustering, Animations)
  animations/    # Custom animation controllers
```

## Design Decisions

- **Provider over Riverpod/Bloc**: Simpler state management suited to the app's complexity level. Providers are composed via MultiProvider at the root.
- **PostGIS for geospatial queries**: Supabase's PostGIS extension enables efficient proximity searches on the activity dataset.
- **OpenStreetMap for geocoding**: Eliminated per-request geocoding costs while maintaining accuracy for US addresses.
- **AI enhancement pipeline**: Crawl4AI (free) + Gemini Flash ($0.0003/park) provides rich facility descriptions at negligible cost — 667x cheaper than initial estimates.
- **Custom map clustering**: Built a force-directed layout manager for marker collision detection rather than using a generic clustering library.
- **Platform-adaptive maps**: Google Maps on Android/Web, with Apple Maps option on iOS via long-press on the Map tab.

## Content Enhancement Pipeline

The app includes an automated pipeline for enriching activity data:

1. **Web Scraping** (Crawl4AI) — Scrapes 6 sources per park simultaneously from official park district sites and specialty sites like Pickleheads
2. **AI Analysis** (Gemini 1.5 Flash) — Extracts facility counts, surface types, hours, and amenities
3. **Deduplication** — Smart matching to prevent duplicate activity entries
4. **Database Update** — Enriched data stored in Supabase with enhancement tracking

## Running Locally

```bash
# Prerequisites: Flutter SDK 3.32+, Dart 3.8+

# Clone and install dependencies
cd sports_finder_app
flutter pub get

# Run on iOS Simulator
flutter run

# Build for web
flutter build web --release
```

## License

This project is not open source. Source code is in a private repository. This public repo serves as a portfolio showcase.

---

Built with Flutter, Supabase, and Gemini AI.
