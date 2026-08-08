# J.A.R.V.I.S. — Android background assistant

Ye ek Android app project hai (source code). Main yahan se seedha APK compile nahi
kar sakta (mere paas is environment mein internet access nahi hai jo Android SDK
download karne ke liye chahiye) — lekin neeche ek tarika hai jisme **GitHub tumhare
liye free mein cloud pe APK build kar dega**, bina tumhare computer/Android Studio
ke.

## Ye app kya karta hai

- Screen off ho ya app background mein ho, phir bhi **"Jarvis" bolte hi sun leta hai**
  (Picovoice Porcupine on-device wake-word engine — koi internet nahi lagta wake word
  sunne ke liye, bilkul offline).
- Wake word ke baad tumhara command sunta hai (Android ka speech-to-text — is step ke
  liye internet chahiye).
- Command Claude API ko bhejta hai, jawab wapas TTS (text-to-speech) se bolta hai.
- Phir wapas chup-chap "Jarvis" ka wait karne lag jata hai.

## Setup — step by step

### 1. Do free API keys chahiye honge

**Picovoice AccessKey** (wake-word engine ke liye, free tier available):
1. https://console.picovoice.ai pe account banao
2. Dashboard se apna "AccessKey" copy karo

**Anthropic API key** (Claude ko baat karne ke liye):
1. https://console.anthropic.com pe account banao
2. API keys section se ek naya key banao
3. **Note:** ye claude.ai (chat) wala subscription nahi hai — ye alag "developer"
   account hai jisme thoda sa credit daalna padta hai (pay-as-you-go, bahut sasta hai
   normal use ke liye — paise ka istemaal hone par hi charge hota hai).

### 2. APK banao — bina computer/Android Studio ke (GitHub Actions)

Isme sirf ek free GitHub account chahiye aur browser (phone se bhi ho jayega):

1. https://github.com pe free account banao (agar nahi hai)
2. Upar right corner "+" → **"New repository"** → koi bhi naam do (e.g. `jarvis-app`)
   → **Public** rakho → "Create repository"
3. Naye khali repo page pe **"uploading an existing file"** link pe click karo
4. Is `JarvisApp` folder ke **andar ka poora content** (files + folders — `.github`,
   `app`, `build.gradle`, `settings.gradle`, `gradle.properties`, `README.md` — sab
   kuch, `JarvisApp` folder khud nahi, uske andar wala saara maal) drag-drop karo
   upload box mein
5. Neeche "Commit changes" dabao
6. Ab repo ke **"Actions"** tab pe jao — ek build automatically shuru ho jayega
   ("Build Jarvis APK" naam se), 3-5 minute lagenge
7. Build complete hone ke baad usi run ke page pe neeche **"Artifacts"** section mein
   `jarvis-debug-apk` milega — usse download karo (ek .zip milega, usme APK hai)
8. Phone mein woh APK file kholo → install karne do (Chrome/file manager "install
   from unknown source" permission maangega, allow kar dena — ye normal hai
   non-Play-Store apps ke liye)

### 2b. (Agar kabhi computer mile) — Android Studio se seedha

1. [Android Studio](https://developer.android.com/studio) install karo
2. "Open" karke ye poora `JarvisApp` folder select karo, Gradle sync hone do
3. Phone USB se connect karo (Developer Options + USB Debugging on), Run dabao

### 3. Phone pe app set up karo

1. App kholo
2. Dono keys paste karo (Picovoice + Anthropic), "Save keys" dabao
3. **"Disable battery optimization"** button zaroor dabao — warna Android kuch der
   baad service ko khud band kar dega (ye sabse common issue hota hai background
   apps ke saath)
4. "Start Jarvis" dabao, mic + notification permission allow karo
5. Ab screen off karke bhi "Jarvis, what time is it?" jaisa bol ke try karo

## Important limitations — sach mein bata raha hoon

- **Battery**: background mein continuous mic listening battery use karta hai.
  Porcupine khud bahut low-power hai (isi liye design hua hai), lekin phir bhi
  kuch phones (Xiaomi/Oppo/Vivo especially) apne "battery saver" se aggressive
  apps ko kill kar dete hain — battery optimization disable karna zaroori hai,
  aur kabhi-kabhi phone-specific "autostart" settings bhi on karni padti hain.
- **Speech-to-text** (command samajhna) internet maangta hai — sirf wake word
  detection offline hai.
- Ye APK Play Store pe published nahi hai (sirf tumhare phone pe directly install
  hoga via Android Studio) — isliye Play Protect kabhi warning de sakta hai,
  "install anyway" choose kar sakte ho, ye safe hai kyunki code tumhare paas hai.
- Agar chahiye toh Hindi commands ke liye bhi bol sakta hoon kaise add karein
  (Porcupine + speech recognizer dono multi-language support karte hain).

## Files

- `app/src/main/java/com/jarvis/assistant/JarvisService.kt` — background listener
  (Porcupine wake word + speech-to-text + Claude call + text-to-speech)
- `app/src/main/java/com/jarvis/assistant/ClaudeClient.kt` — Claude API call
- `app/src/main/java/com/jarvis/assistant/MainActivity.kt` — settings screen
