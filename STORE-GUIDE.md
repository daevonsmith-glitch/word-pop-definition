# Word Pop: Definition – publishing on Google Play and the App Store

Step by step for both stores, publishing under your own name (personal accounts), including how the game gets built.

**What publishing as a person means:** no D-U-N-S number is needed, but your legal name is shown publicly as the seller on both stores, and Google makes every new game run a 14-day closed test with at least 12 testers before it can go public.

**What's already done:** the game (phones and tablets), the Android build, the iPhone and iPad build, the signing key for Google, the privacy policy page, the store text below, and the graphics for both stores.

---

## Part 0. Before either store (do once)

1. **Fill in the privacy policy.** In `www/privacy.html`, replace `[YOUR NAME]` with your name and `[YOUR CONTACT EMAIL]` with an email players can write to.
2. **Decide the app ID.** It is set to `app.wordpop.definition` in `capacitor.config.json`. Both stores use it as the game's permanent ID, and it can never be changed after the first upload. Keep it, or change it now (for example `com.yourname.wordpopdefinition`).
3. **Put the game on GitHub and Cloudflare** by following `README.md` sections 1 to 3. Both stores need a public link to the privacy policy, which will be `https://YOUR-CLOUDFLARE-LINK/privacy.html`.
4. **Add the iPhone build file to GitHub.** Phone uploads drop the hidden `.github` folder, so add it by hand like the Android one: **Add file → Create new file**, name it `.github/workflows/ios-build.yml`, paste the contents from the end of `README.md`, and commit.

---

## Part 1. Google Play

### 1A. Open the account (once)

1. Go to **play.google.com/console** and sign in with the Google account that should own the games. You must be 18 or older.
2. Choose **Personal** as the account type.
3. Enter a developer name (what players see), your legal name, address, email and phone. Verify the email and phone with the codes Google sends.
4. Pay the **$25** fee. It is charged once, with no renewal.
5. Upload a government photo ID when asked. Personal accounts are verified with an official ID.
6. If asked, confirm you have a real Android phone by signing in to the **Play Console** app on it.

### 1B. Build the Play Store file

1. Open **play-store-signing-key.txt** (kept separately, never in GitHub).
2. In the GitHub repository: **Settings → Secrets and variables → Actions → New repository secret**.
3. Add `KEYSTORE_PASSWORD` with the password from the file.
4. Add `KEYSTORE_BASE64` with the long single line from the file.
5. Go to **Actions → Build Android APK → Run workflow**.
6. After about 5 to 8 minutes, open **Releases**. You get two files:
   - `WordPopDefinition.apk` – install it on your own Android phone to test.
   - `WordPopDefinition.aab` – the file you upload to Google Play.

Every run gives the game a higher version number automatically, which Google requires for each upload.

### 1C. Create the app and listing

1. In Play Console: **Create app** → name `Word Pop: Definition`, language English (United States), **Game**, **Free**.
2. **Store listing:** paste the text from "Store text" below. Upload the Google graphics: `app-icon-512.png`, `feature-graphic-1024x500.png`, the four `screenshot-...png` files under **Phone screenshots**, and the four `tablet-...png` files under both **7-inch tablet** and **10-inch tablet screenshots**. The tablet screenshots let Google show the game as tablet-ready.
3. **App content** forms:

| Form | Answer |
|---|---|
| Privacy policy | `https://YOUR-CLOUDFLARE-LINK/privacy.html` |
| App access | All functionality available without special access |
| Ads | No |
| Content rating | Fill in honestly. See "Rating note" below. |
| Target audience | 18 and over |
| Data safety | Collects or shares user data: **No** |
| Government, financial, health, news | No |

### 1D. The 14-day closed test (required for new personal accounts)

1. **Find 12 testers now.** Each needs a Google (Gmail) account and an Android phone. Friends and family work. If you'll publish all four games, the same 12 people can test all of them; the rule counts for each game separately.
2. **Testing → Closed testing → Create track** (or use the default "Closed testing - Alpha").
3. **Testers:** create an email list with the 12+ Gmail addresses.
4. **Create new release**, upload `WordPopDefinition.aab`, accept **Play App Signing** when asked, and roll it out.
5. Copy the **opt-in link** from the Testers tab and send it to everyone. Each tester opens it, taps **Become a tester**, then installs the game from Google Play.
6. All 12 must **stay opted in for 14 days in a row**. Ask them to open the game a few times. Recruit a few extra in case someone drops out.
7. On day 14 or after, go to **Dashboard → Apply for production** and answer Google's questions about the test (who tested, what feedback you got, what you changed).
8. Google reviews the application, usually within about a week.

### 1E. Go live

1. **Production → Create new release.** Choose the same `.aab` from the library, add release notes ("First release"), pick countries.
2. **Send for review.** Review of a new account's first release usually takes a few days.

---

## Part 2. Apple App Store

### 2A. Open the account (once)

1. Make sure your Apple Account uses **two-factor authentication** and your **legal name exactly as on your ID**.
2. Install the **Apple Developer** app on an iPhone or iPad. Identity verification happens in that app.
3. In the app: **Account → Enroll Now** → choose **Individual**.
4. Enter your legal name, address and phone, then scan your driver's license or other government ID.
5. Accept the agreement and pay **$99 per year**.
6. Wait for the approval email. Your legal name becomes the seller name on the App Store.
7. Sign in at **appstoreconnect.apple.com** and accept the agreements shown on the home page (the free apps agreement is enough for free games).

### 2B. Register the game with Apple

1. Go to **developer.apple.com → Account → Certificates, IDs & Profiles → Identifiers → +**.
2. Choose **App IDs → App**. Description: `Word Pop Definition`. Choose **Explicit** and enter the app ID from Part 0 (for example `app.wordpop.definition`). No extra capabilities are needed. **Register**.
3. In **App Store Connect → Apps → + → New App**:
   - Platform: iOS
   - Name: `Word Pop: Definition`
   - Primary language: English (U.S.)
   - Bundle ID: the one you just registered
   - SKU: `wordpopdefinition` (any private label)
   - User access: Full access

### 2C. Connect GitHub to your Apple account (once)

GitHub's Mac machines build and sign the iPhone app, so you don't need a Mac. They need four secrets.

1. **Team ID:** developer.apple.com → **Account → Membership details**. Copy the 10-character **Team ID**.
2. **API key:** App Store Connect → **Users and Access → Integrations → App Store Connect API**. If you see **Request Access**, tap it and accept; it's usually granted right away.
3. Under **Team Keys**, tap **+**, name it `GitHub`, set access to **Admin**, and **Generate**. Admin is needed so the build can create Apple's signing certificate for you.
4. Copy the **Issuer ID** (shown above the list) and the **Key ID** (in the key's row).
5. **Download API Key.** You get a file named `AuthKey_XXXXXXXXXX.p8`. Apple lets you download it **only once**, so save it somewhere safe and private.
6. Open the `.p8` file in a text editor and copy all of it, including the `BEGIN` and `END` lines.
7. In GitHub: **Settings → Secrets and variables → Actions → New repository secret**, and add:

| Secret name | Value |
|---|---|
| `APPLE_TEAM_ID` | the Team ID |
| `ASC_ISSUER_ID` | the Issuer ID |
| `ASC_KEY_ID` | the Key ID |
| `ASC_KEY_P8` | the whole contents of the `.p8` file |

The same key and secrets work for all four games; each game's GitHub repository needs its own copy of the secrets.

### 2D. Build and test

1. GitHub → **Actions → Build iPhone app → Run workflow**. It takes about 10 to 20 minutes.
2. When it shows a green check, the game has been sent to App Store Connect. Apple processes it for another 15 to 30 minutes.
3. App Store Connect → your app → **TestFlight**. The build appears there. Under **Internal Testing**, create a group, add yourself, and select the build.
4. Install the **TestFlight** app on your iPhone, open the invite, and install the game. Play each mode and check that words are read aloud (silent switch off).

Each run gives the build a higher number automatically.

### 2E. Store listing

In App Store Connect → your app → the **1.0 Prepare for Submission** page:

1. **Screenshots:** the game runs on both iPhone and iPad, so Apple needs both sets. Upload the four `iphone-...png` files to the **iPhone 6.9" display** slot and the four `ipad-...png` files to the **iPad 13" display** slot.
2. **Text:** paste the App Store text from "Store text" below.
3. **Support URL:** your Cloudflare link. **Privacy Policy URL:** the privacy page link (under **App Privacy**).
4. **Build:** tap **+** and select the build that came through TestFlight.
5. **App Review Information:** your name, phone and email. No sign-in is needed, so leave the demo account empty.
6. **Age Rating:** answer the questionnaire honestly. See "Rating note" below.
7. **App Privacy:** **Get Started** → "Do you or your third-party partners collect data from this app?" → **No** → Publish.
8. **Pricing and Availability:** Free, choose countries.
9. **App information → Category:** Games, subcategory Word (secondary: Trivia or Educational).

### 2F. Submit

1. Tap **Add for Review**, then **Submit to App Review**.
2. Apple usually replies within a day or two, but there's no guarantee. If they reject it, the message says why; send it to me and I'll fix the issue.

---

## Store text

### Google Play

**App name:** `Word Pop: Definition`

**Short description** (80 max):

    Pop the bubble with the right meaning. 1,200 words. Solo, battle and teams.

### App Store

**Name:** `Word Pop: Definition`

**Subtitle** (30 max): `Pop the right meaning`

**Keywords** (100 max, commas, no spaces):

    vocabulary,definitions,word,game,dictionary,trivia,bubble,brain,party,multiplayer,quiz,SAT,GRE

**Promotional text** (optional): `1,200 words across four levels, from everyday to almost nobody knows.`

### Full description (both stores)

    Word Pop: Definition is a vocabulary game for adults. A word appears at the top of the screen, color-changing bubbles float up carrying possible meanings, and you pop the one that fits.

    FOUR LEVELS, 1,200 WORDS
    • Easy – everyday words
    • Medium – college-level words
    • Hard – words that win arguments
    • Extreme – words almost nobody knows
    Each level has 300 words, shuffled every time, and you see all of them before any repeat.

    THREE WAYS TO PLAY
    • Solo – how many can you get before your third miss?
    • Battle – 2 to 4 players pass one phone and take turns. Three misses and you're out. First to 10 wins.
    • Teams – 2 against 2, sharing a score and three misses.

    FAIR CHOICES
    The wrong answers are always the same kind of word as the right one and about the same length, so you can't guess from how a definition is worded. You have to know the word.

    LEARN AS YOU PLAY
    Every word and its meaning is shown and read aloud after each round, whether you got it right or not.

    CALM AND SIMPLE
    No timer, no ads, no accounts and no purchases. Calm mode slows the bubbles down. Works offline.

---

## Rating note

Neither store lets you pick the rating. It comes from your answers to a questionnaire, and the answers must be true. The Extreme level has seven words whose dictionary meanings touch rated topics:

- Sexual references: CALLIPYGIAN ("having shapely buttocks"), ECDYSIAST ("a striptease performer")
- Alcohol or drugs: BIBULOUS ("fond of drinking"), CRAPULOUS ("sick from too much drink"), NEPENTHE ("a drug of forgetting")
- Violence: FUSTIGATE ("to beat with a club"), EVISCERATE ("to disembowel")

They're plain definitions with nothing graphic. Answer the questions on those topics with them in mind (on Apple's form these are usually "infrequent/mild"). The rating each store shows at the end is the real result.

---

## Files

| File | For |
|---|---|
| `word-pop-definition.zip` | The game, both build files, this guide |
| `play-store-signing-key.txt`, `upload-keystore.jks` | Google signing key. Private: never in GitHub. |
| `app-icon-512.png`, `feature-graphic-1024x500.png`, `screenshot-1` to `-4` | Google Play listing (phones) |
| `tablet-1` to `-4` | Google Play listing (tablets, 1440 × 2560) |
| `iphone-1` to `-4` | App Store listing (iPhone, 1290 × 2796) |
| `ipad-1` to `-4` | App Store listing (iPad, 2048 × 2732) |
| `AuthKey_XXXXXXXXXX.p8` | Apple API key you download in 2C. Private: never in GitHub. |

## If a build fails

**Actions** → the failed run → the job → the step with the red ❌. Send me the step's name and the red lines near the bottom. The iPhone build's signing step is the one most likely to need adjusting on its first run.

## Costs

| | Google Play | App Store |
|---|---|---|
| Account | $25 once | $99 per year |
| Per game | Free | Free |
| GitHub builds | Free for public repositories | Free for public repositories |
