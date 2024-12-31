Mafuyu Rewrite 2.0 (Unreleased)

[Main]
- Refactor all structure
- Beautify variable & code 
- 100% Rewrite with efficiency

[Voice States Event]
- (Fitur Auto Leave untuk Efisiensi) - exclude premium user (will immune)
  -> All in one process -> yg artinya 1 proses bisa buat menampung banyak guild tanpa perlu spawn process job lagi
  (Kondisi idle Not Playing)
  -> Ketika mafuyu join VC tapi **not playing `is_playing()`** bakal set timer 3 menit untuk leave otomatis dalam 1 process (dictionary masing masing guild&channel)
  -> Ketika mafuyu di move dari VC A to VC B dan kondisi nya **"not playing `is_playing()"** timer dari channel sebelumnya dipertahankan cuma replace `channel_id`
  -> Add unhandled event: semisal mafuyu join VC A tapi state nya gak kedetect entah karena race cond atau discordapi nya gak return data yg sesuai, Pas mafuyu di move ke channel B, nanti bakal otomatis di catet ulang dan masuk ke timer 3 menit (anggap aja ini pas race cond atau discord gak ngasih return data)
  -> Add support kill process job: kalo di dalem dictionary bener bener gada yang play task nya otomatis di kill (cleanup) untuk hindari mem,cpu leak issue
  (Kondisi saat Mafuyu Playing)
   -> Ketika mafuyu join VC dan playing radio maka timer akan freeze & immune dari hitung mundur (Unlimited Session)
- (Fitur Auto Speak on Stage)
  -> Mafuyu otomatis naik stage kalo perms nya memenuhi stage channel tsb
- (Fitur Pause & Resume)
  -> Ketika mafuyu lagi playing `is_playing()` tapi di mute server = Mafuyu otomatis pause player & continue resume saat mute server di lepas

[Mafuyu Protection]
- Add support anti theft -> "Leave secara diam diam di channel lain akan terdeteksi"

[Guild Channel Update Event]
- Enabled by default Notif (Channel Name, Slow Mode, Age Restrict, Bitrate, Video Quality, User Limit, Region Override)
- Add support send text biasa kalo embed_link server gak hidup
- Pass jika no permission

[Commands & Protection Handle]
- Implement semua command dengan proteksi rate limit static (per user 3 command, cooldown 10 detik)
- Required Channel Perms: View Channel, Manage Channels, Connect, Speak, Use Voice Activity, Priority Speaker, Send Message, Embed Links, Use External Emoji, Use Application Command, Read Message History
- Permission Requirements Protect: Semisal mafuyu kekurangan perms dalam channel nanti bakal di infoin lewat channel (fallback: DM or pass krn gbs sama sekali)

[Radio Player Handle]
- Auto Endpoint Check: sebelum radio di play bakal receive status dulu apakah url stream nya 200 ok atau mati timed out (fallback: send notif channel radio is down or not available at this moment)
- Auto Rewrite URL Stream = terkadang beberapa hari dari pusat di refresh maka dari itu auto rewrite ke db
- Use message edit + animation process
- Add footer + image url if avail
- Automatic Reconnect & Resume Session Player (Premium User) : mafuyu mampu reconnect otomatis jika bot di restart,mulai ulang atau saat playing terjadi gangguan putus yang bukan disebabkan sama fungsi `disconnect()` ataupun di timeout sama admin nakal

[Embed & Style]
- All command fully migrated Embed semua di lengkapi footer Guild Name, Guild Icon, Timestamp
- Add emoji button untuk "Your server is not registered"
