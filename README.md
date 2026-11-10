# Goldkartei Voice Packs

Recordings that give [Goldkartei](https://github.com/karteien/goldkartei)'s German vocabulary
decks sound: with a voice pack, every card says its word aloud, and the back of the card has a
button to hear it again.

A **voice pack** is one voice's recordings of every word in the decks. There are five:

| Voice pack | Voice | Accent |
|---|---|---|
| `de-at/aws-hannah` | Hannah (Amazon Polly) | Standard Austrian German |
| `de-de/aws-daniel` | Daniel (Amazon Polly) | Standard German as spoken in Germany |
| `de-de/aws-lennart` | Lennart (Amazon Polly) | Standard German as spoken in Germany |
| `de-de/aws-tina` | Tina (Amazon Polly) | Standard German as spoken in Germany |
| `de-de/aws-vicki` | Vicki (Amazon Polly) | Standard German as spoken in Germany |

A deck is built with one voice pack, and its file name says which: `Goldkartei v0.3 [de-at Hannah].apkg`.

## How to use a voice pack

You need two things first:

- **git**, to download this repository ([git-scm.com](https://git-scm.com/downloads)).
- **A copy of Goldkartei with its program built**, since a deck with sound is one you build
  yourself. Goldkartei's [developer guide](https://github.com/karteien/goldkartei/blob/master/docs/DEVELOPERS.md)
  explains how.

Then, in a terminal:

1. **Download the voice packs** into the folder that holds your copy of Goldkartei, so the two
   sit side by side:

   ```sh
   git clone https://github.com/karteien/goldkartei-voice-packs.git
   ```

2. **Pick the version that matches your Goldkartei.** The recordings change when the words do,
   so they must come from the same version as the Goldkartei you build:

   - If you build the newest Goldkartei, you already have the newest recordings. Skip this step.
   - If you build a release, such as Goldkartei `v0.3`, switch to the same version here:

     ```sh
     git -C goldkartei-voice-packs checkout v0.3
     ```

3. **Build the deck** from inside your Goldkartei folder, naming the voice pack you want:

   ```sh
   goldkartei build --with-voice-pack ../goldkartei-voice-packs/de-at/aws-hannah
   ```

   The deck appears in Goldkartei's `dist/` folder, with the voice's name at the end of its file
   name. Import it into Anki as usual. If a recording is missing or doesn't match a word, the
   build stops and says which, which usually means the versions in step 2 don't match.

To get newer recordings later, update the download and build again:

```sh
git -C goldkartei-voice-packs pull
```

or, for a release, fetch its version and switch to it:

```sh
git -C goldkartei-voice-packs fetch --tags
git -C goldkartei-voice-packs checkout v0.4
```

### Using one voice pack

The steps above download every voice pack, along with every earlier version of every
recording. That is the simplest way, but the download grows with each release. If you know
which voice you want and need only one version, git can download just that: one voice pack, as
it is in that version, and nothing else. That is a fraction of the size and much quicker.

Instead of step 1, run these two commands, with the version you build after `--branch` (or
`master` for the newest):

```sh
git clone --depth 1 --branch v0.3 --filter=blob:none --sparse https://github.com/karteien/goldkartei-voice-packs.git
git -C goldkartei-voice-packs sparse-checkout set de-at/aws-hannah
```

What the options do:

- `--depth 1` leaves out the earlier versions.
- `--filter=blob:none` and `--sparse` download no recordings at first.
- `sparse-checkout set` then fetches the recordings of the voice pack you name, and only those.

Then build as in step 3. To move to another version later, fetch only that version:

```sh
git -C goldkartei-voice-packs fetch --depth 1 origin tag v0.4
git -C goldkartei-voice-packs checkout v0.4
```

To add a second voice pack, name it too:

```sh
git -C goldkartei-voice-packs sparse-checkout add de-de/aws-daniel
```

## Technical details

Each voice pack is a folder `<variety>/<vendor>-<name>`. The variety is the pronunciation the
voice speaks, as a lowercased language tag (`de-at`, `de-de`), and a deck built with it carries
the variety and the voice's name at the end of its file name (`[de-at Hannah]`).

Each recording is an MP3 named after what its voice was given (a UUID v5): the word as it is
written, or, for the few words spelled alike but said differently (*Band*), SSML with the word's
IPA. Its ID3v2.4 tag records the text, the IPA, that input and the voice. Goldkartei's data names
the recording each entry plays, and its build checks every recording against its tag. The recordings
are made from that data by Goldkartei's `tools/pronunciation/generate.py`; Goldkartei's
`sources/voice/README.md` explains the naming and the tags.

The tags of this repository follow Goldkartei's: `v0.3` here holds the recordings that go with
Goldkartei `v0.3`, and `master` has the newest.

## Sources and credits

- The voices read Goldkartei's words as they are written. For the few words spelled alike but
  said differently, they follow Goldkartei's IPA, and every recording's ID3 tag records the
  word's IPA: largely from [Wiktionary](https://www.wiktionary.org/) (German and English
  editions), by Wiktionary contributors, licensed under
  [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/), adapted by Goldkartei's rules
  for Standard Austrian German and completed by hand and by model where Wiktionary had none.
- The voices are Amazon Polly's (Hannah, Daniel, Lennart, Tina and Vicki, generative engine).
  Amazon Polly is a trademark of Amazon.com, Inc. or its affiliates; this repository is not
  affiliated with or endorsed by Amazon.

## License

The Goldkartei Contributors dedicate these recordings to the public domain under
[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/); see `LICENSE`. You may
copy, change, share and use them for any purpose, commercial or not, without asking and without
giving credit.

CC0 covers only the rights The Goldkartei Contributors hold. It doesn't cover the names Goldkartei
or Amazon Polly, or any rights others may have in the IPA, such as the part that comes from
Wiktionary (see above).

Credit isn't required, but it's appreciated:

> "Goldkartei Voice Packs" by The Goldkartei Contributors,
> https://github.com/karteien/goldkartei-voice-packs, CC0 1.0.
