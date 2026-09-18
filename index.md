---
title: "Privacy Policy — Zermelo Homework"
description: "What the Zermelo Homework iOS app stores, where, and what leaves your phone."
---

# Privacy Policy — Zermelo Homework

*Last updated: 2026-09-18*

This app shows you your school timetable and, above all, your homework. It talks to your own
school's Zermelo portal and to nothing else.

This page is not a generic privacy policy. It is a plain-language transcription of an internal
document that lists, item by item, every piece of data the app actually holds, where it holds it,
and what leaves your phone. That document was written from the shipping source code rather than
from intentions, and it is named at the bottom of this page so you can check this text against it.

Read this as the honest version rather than the reassuring one. Some of what follows — that your
school password is kept on your device — is the kind of thing a policy is often written to hide.

## What this app stores on your device

Two places on your phone, and nowhere else.

**The iOS Keychain**, the part of iOS built to hold secrets, holds five things: the short label of
your school (what you type before the rest of the portal address), your Zermelo username, your
Zermelo password, the access token your school's portal issues after you sign in, and a random
value the app generates on your device to mark which files belong to which account.

All five are stored with the protection class
`kSecAttrAccessibleAfterFirstUnlockThisDeviceOnly`. In plain terms, that means:

- they stay on this one device — they are excluded from iCloud Keychain, so they do not sync to
  your other Apple devices, and they are excluded from encrypted device backups;
- they are readable only after you have unlocked the phone at least once since it was switched on;
- they carry no Keychain access group, so no other part of the app bundle — including the home
  screen widget — can read them, as a matter of how the app is built rather than of good manners;
- they are never written to a log. An automated check refuses to build the app if any logging or
  printing call anywhere in it could reach a password or a token.

**Your school password is stored.** This is unusual and it is deliberate, so here is the reason
rather than a reassurance. Zermelo's portal issues no refresh token, and a real token measured
against a real school expires after 16 hours, hard, with no extension for being active. Signing in
again is therefore a daily event for every real user. Storing your password is the only thing
between you and retyping your school credentials every single day. The protections above reduce the
risk that carries. They do not remove it.

**The app's shared container**, an area named `group.nl.fersoft.zermelo` that the app and its home
screen widget both read, holds three files. All three are written with iOS file protection set to
`.completeFileProtectionUntilFirstUserAuthentication`, so their contents are encrypted until you
first unlock the phone:

- **Your cached timetable**, one file per school week. It holds the lessons exactly as your school
  returned them — start and end times, subject codes, teacher codes, room codes, group codes,
  period numbers, the changed and cancelled flags, and the homework text your teachers wrote. This
  is what lets the app show you your homework with no signal. It contains no password, no
  username, no token and no school name; an automated test searches the written file for each of
  those.
- **Your homework ticks and your own due dates.** Which homework you marked done, and the dates
  you set yourself for when you plan to finish something. This is yours; none of it is ever sent to
  Zermelo or anywhere else. When you untick something and it has no due date left, its record is
  deleted rather than kept as a trace of what you undid.
- **A record of which schedule changes you have already been told about**, so the app does not
  announce the same cancelled lesson twice, plus whether you turned change alerts on and whether
  you dismissed the card offering them.

Two of those files keep a fingerprint rather than the text it came from: a `SHA-256` value of the
homework text as it read when you ticked it, and a `SHA-256` value of a schedule change as it was
described to you. Being straight about what a fingerprint does — a `SHA-256` value cannot be turned
back into the text, but anyone who already has a guess at the text can check whether it matches. In
this case that reveals nothing extra, because the real text is sitting in the cached timetable file
right next to it.

The random account marker is a freshly generated random identifier. It is not derived from your
name, your username, your password or your student number, so nothing about you can be worked out
from it. It exists so that if a different student signs in on the same phone, they cannot be shown
your cached weeks or your homework ticks.

If you use the app in demo mode without signing in, it renders from a sample week built into the
app itself. Nothing is written to the container, and the ticks you make in demo mode are gone when
you quit.

## What leaves your device

**One destination: `https://{school}.zportal.nl`** — your own school's Zermelo portal, over HTTPS,
where `{school}` is the label you typed when you linked the app. Nothing goes anywhere else.

There is no server operated by this app's developer, anywhere in this system. That is an
architectural rule the app was built under, not a temporary state of affairs — it is what keeps
other students' data off infrastructure the developer would be responsible for. It is also not the
answer to the question of what the app stores; that question is answered in the section above,
where the honest answer lives.

These are all the requests the app ever makes, every one of them to your school's portal:

- starting a sign-in, to fetch your school's own login form;
- sending your username and password to sign you in, and again each day when the token expires.
  They go in the body of the request, never in the address, because addresses are recorded verbatim
  in proxy and server logs;
- exchanging the resulting code for an access token;
- asking your school to revoke the token when you unlink;
- reading your timetable for a week;
- reading your own student number once per session, to label the cached file;
- reading your school's own subject names and teacher names once per session.

The address is assembled by exactly one piece of code in the app, which accepts only a plain school
label and appends the rest itself. A pasted address, a port number or a path cannot become a
destination.

Your homework ticks, your own due dates and the record of changes you were notified about are never
transmitted. No request above carries any of them.

## What this app never receives

**Your own full name.** Your school's portal will hand it back with the subject and teacher names
if asked, and the app does not ask: the `students` field is left out of that request, so your name
never enters the app at all. An automated test checks the exact list of fields the app asks for.
This is worth stating precisely because it is the strongest promise on this page — a name the app
never receives cannot be cached, logged or disclosed.

Held only in memory, never written to disk: your school's subject names and its teachers' full
names, fetched once each time you use the app, and the session cookie your school's login form
uses, which is discarded when the app closes.

Never received at all, and nowhere to put it: anything about your classmates beyond the group codes
printed on your own lessons.

## How long it is kept, and how to erase it

Your cached timetable is kept one file per school week, refreshed when you open the app or when iOS
lets the app check in the background. The record of changes you were told about keeps a rolling
month — entries for lessons that started more than 28 days ago are dropped on every check, and week
markers older than four school weeks go with them. Your homework ticks and due dates are kept for
as long as you keep them, because they are your record of your own work.

**Unlinking erases it.** One action in the app deletes the Keychain items listed above, every
cached week, your homework ticks and due dates, and the record of notified changes, and asks your
school to revoke the access token. If any part of the local erasure fails, the app tells you so
rather than claiming success. Removing delivered notifications from your lock screen is part of the
same step.

**Deleting the app erases it too.** iOS removes the Keychain items and the shared container when
the app is deleted, without the app being involved.

**Signing in as a different student erases the previous account's data first.** That happens before
the new account is stored, and if anything could not be removed the sign-in stops rather than
continuing.

## Notifications and your calendar

Change notifications are created on your device. They are ordinary local iOS notifications that the
app adds itself after comparing a freshly fetched week against the one it had. There is no push
service, no Apple Push Notification registration, no device token and no server: nothing about a
notification leaves your phone.

A notification's title names the subject, the weekday and the time; its body names what changed —
a new time, a new room, a different teacher, or that the lesson was cancelled. The free-text change
description your school's system sometimes carries is never used. The hidden data attached to a
notification is only numbers: the week, the lesson's identifier and its start time. If your Show
Previews setting hides notification contents on the lock screen, iOS shows "Your schedule changed"
instead of the lesson details.

One rough edge, stated because it shows on a lock screen: when iOS wakes the app in the background,
there is too little time to also fetch your school's teacher-name table, so a background "Teacher
changed to …" notification may show your school's raw teacher code rather than a name. That code is
already in the cached timetable on your device, so nothing new is disclosed — but it is what you
will see.

Adding lessons to your calendar writes events into your own default calendar on your own device and
sends nothing anywhere. The app asks for write-only calendar access, so it cannot read what is
already in your calendar.

## No analytics, no advertising, no third parties

No analytics service. No crash reporter. No advertising identifier. No third-party software
development kit of any kind. No tracking, and no tracking domains — the app ships a privacy
manifest in both the app and the widget saying exactly that.

This is not a promise about intent; it is checked against the app that gets submitted. An automated
gate opens the built app and asserts that every library it links comes from iOS itself, that it
embeds no third-party framework, that nothing inside the compiled binary so much as names any of
the common analytics and crash-reporting services, and that the same holds for the home screen
widget as a separate program. The same gate confirms the app carries no Keychain sharing
permission, which is what makes the widget structurally unable to read your token.

## Who this app is for

Dutch secondary school students, many of them minors. That is the constraint behind every choice
above: the omitted name field, the file protection on the cache, the absence of any server, the
absence of analytics, and unlinking being a real erasure rather than a sign-out.

If you are a parent or guardian and want to understand or remove what is on your child's phone,
unlinking inside the app or deleting the app both erase everything listed on this page, and neither
requires contacting anyone.

## Legal review

This app **has not undergone a formal legal compliance review**. The technical posture described on
this page was designed to make such a review short, and it is not a substitute for one. Nothing on
this page, and nothing in the app's source repository, is a compliance assessment, and no claim of
compliance with the GDPR, the AVG or any other regime is made anywhere in it. Whether and how those
regimes apply to an independent developer shipping an on-device client that touches minors' data is
an open question that a lawyer, not a developer, should answer.

That caveat is published here rather than kept internal because you are entitled to know which
assurances have been checked by a professional and which have not.

## Contact

Questions about this policy, or about data on a specific device, go to the support contact listed on
this app's App Store page, which is the contact of record. The app's source repository — the same
repository that publishes this page — also accepts public issues, which is the better route if you
want the answer to be visible to other students and their parents.

If you want data on a device erased and do not want to wait for a reply: unlink inside the app, or
delete the app. Both are described under "How long it is kept, and how to erase it" above, and
neither needs anyone's help.

## Source

This page is a transcription of
`.planning/phases/01-linked-school-live-lessons/01-DATA-INVENTORY.md` in this app's source
repository — the internal data inventory written from the shipping code at the end of each phase of
development. Every factual claim above corresponds to a row or a paragraph of that document. Where
the two ever disagree, `01-DATA-INVENTORY.md` is the one written against the code, and this page is
the one that needs fixing.

Transcribed from commit db8ffb2.
