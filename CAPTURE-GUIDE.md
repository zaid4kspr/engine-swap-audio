# Teaching the app your adapter's language (Ancel BD500 etc.)

Only needed if the app says **"does not answer ELM327 commands"**.

We record the Bluetooth traffic between the phone and the adapter while **Ancel's own app**
shows live RPM. A tool then works out the commands and where RPM sits in the replies, and
produces an *adapter profile* you paste into the engine-sound app.

## 1. Turn on Bluetooth recording (once)

1. **Settings → About phone → Software information** → tap **Build number** 7 times
   (enables Developer options).
2. **Settings → Developer options** → turn on **Enable Bluetooth HCI snoop log**
   (if it offers a choice, pick **Enabled** / **Full**, not "Filtered").
3. Turn **Bluetooth off and on again** (recording starts only after this).

## 2. Record (in the car, about 1 minute)

1. Ignition on, engine **running**.
2. Open **Ancel's app**, connect to the BD500, and open the **live data / data stream** screen for the
   **engine**, showing **Engine RPM**. (Fewer values selected = cleaner recording.)
3. Let it idle 10 s, then **rev to about 3000 rpm and back, 3–4 times**, with a few seconds between.
4. Close Ancel's app.

## 3. Get the file off the phone

**Samsung:** open the phone dialer, type `*#9900#` → **Run dumpstate/logcat** (wait) →
**Copy to sdcard (include CP Ramdump)**. The file is in `Internal storage/log/` and named
something like `btsnoop_hci.log` (sometimes inside a `.zip`).

**Other Android phones:** Developer options → **Take bug report** → **Full report** → share the zip.
Inside it: `FS/data/log/bt/btsnoop_hci.log`.

You can leave the file on the phone. No PC is needed.

## 4. Load it into the app (on the phone)

1. Open the engine-sound app → More tab → **Advanced: connection log & adapter settings** → **📂 Load Bluetooth capture**.
2. Pick the `btsnoop_hci.log` file, or the whole bug-report `.zip`. The app finds the log inside.
3. The log shows what kind of protocol it is. If it finds RPM, it says
   **"✅ Adapter profile created and saved"**.
4. Close Ancel's app → tap **Connect OBD (Bluetooth)** → pick the BD500.

If something fails, tap **Copy log** and send it to Claude.

*(PC alternative: `python tools/btsnoop_analyze.py btsnoop_hci.log --out report.txt` writes a
detailed report plus `report.profile.json`.)*

Afterwards turn **Enable Bluetooth HCI snoop log** off again.
