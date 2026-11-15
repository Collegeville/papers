# Papers Starter

A starter site for a collection of papers, each with a short pitch video and a "Take the Challenge" quiz. Copy this repo to make a site like it for your own papers. You can see it running at **https://collegeville.github.io/papers/**, and a full collection built the same way at [maherou.github.io/papers](https://maherou.github.io/papers/).

Papers are archived somewhere permanent, such as Zenodo with a DOI, and videos are on YouTube. This repo holds the page that brings them together, the quizzes, the quiz player, and two Claude skills that help make the quizzes and videos. The one example paper is *A Better Lesson* by Michael A. Heroux.

## Make your own copy

You need a free GitHub account and [GitHub Desktop](https://desktop.github.com/).

1. **Copy the repo.** At the top of this repo's GitHub page, click **Use this template**, then **Create a new repository**. Choose your account as the owner, name it `papers`, make it **Public**, and click **Create repository**.
2. **Get it on your computer.** In GitHub Desktop, choose **File > Clone Repository**, pick your new `papers` repo, and click **Clone**.
3. **Make it yours.** Open the folder (in GitHub Desktop: **Repository > Show in Finder**) and edit these files in any text editor:
   - `_config.yml`: set `title` (your name), `intro`, `description`, `home_label` and `home_url` (a link to your home page, or delete both lines), and `copyright`. Set `url` to `https://YOUR-ACCOUNT.github.io`. Delete the `starter_note` line.
   - `LICENSE` and `LICENSE-CONTENT`: add your name to the copyright lines (see License below).
   - `README.md`: replace this text with a few lines about your collection.
4. **Save your changes to GitHub.** In GitHub Desktop, type a short summary such as "Make it mine", click **Commit to main**, then **Push origin**.
5. **Turn on the website.** On your repo's GitHub page, go to **Settings > Pages**. Under **Build and deployment**, choose **Deploy from a branch**, pick **main** and **/ (root)**, and click **Save**. After a minute or two the site is live at `https://YOUR-ACCOUNT.github.io/papers/`.
6. **Add your first paper** (below). When it's in, delete the example: its entry in `_data/papers.yml`, the two `assets/papers/a-better-lesson*.jpg` images, and the `quizzes/a-better-lesson` folder. Commit and push again.

Each time you commit and push in GitHub Desktop, the site updates within a couple of minutes.

## What's here

```
index.html                  the papers page
_data/papers.yml            one entry per paper
assets/papers/              cover and video thumbnails
quizzes/<slug>/quiz.json    one quiz per paper
player/                     the quiz player (plain HTML and JavaScript)
skills/article-quiz/        Claude skill: paper PDF -> quiz.json
skills/article-video/       Claude skill: paper -> narrated pitch video
_config.yml                 site title, intro text, and address
```

## Adding a paper

1. Add an entry to `_data/papers.yml`. The page lists papers newest month first, and alphabetically by title within a month. The comments at the top of that file explain each field.
2. Save the paper's first page as its cover image, `assets/papers/<slug>.jpg`:

   The image should be about 680 pixels wide. You can save it from any PDF viewer, or ask Claude to make it with:

   ```bash
   pdftoppm -f 1 -l 1 -r 80 -jpeg -jpegopt quality=82 -singlefile paper.pdf assets/papers/<slug>
   ```

3. Optional: if YouTube's thumbnail is not the frame you want, save one from the video and set `thumbnail` on the entry. Choose a time when the title slide has finished appearing:

   ```bash
   ffmpeg -ss 4 -i video.mp4 -frames:v 1 -vf scale=540:-2 -q:v 3 assets/papers/<slug>-video.jpg
   ```

4. For a paper already published elsewhere, set `url` (where "Read paper" goes) in place of `doi`, `venue` for the journal citation, and `related` for other versions (each with `label`, `url` and an optional `note`). Set `video.doi` if the video has its own Zenodo record. If the cover image is the publisher's page, set `cover_note` to say so and add it to the "Not covered" list in `LICENSE-CONTENT`.
5. Optional: add a quiz (below) and set `quiz: true` to show the "Take the Challenge" link.

## Making a quiz

1. In the Claude app, with the `article-quiz` skill installed (see below), attach the paper PDF and ask for a quiz. Give the PDF URL and pitch-video URL if you have them.
2. Review the questions in the conversation, or in `quiz-summary.md`.
3. Save the files to `quizzes/<slug>/`, using the paper's slug from `_data/papers.yml`.
4. Check the quiz against the PDF. It must print `OK`:

   ```bash
   python3 skills/article-quiz/scripts/validate_quiz.py quizzes/<slug>/quiz.json --pdf path/to/paper.pdf
   ```

   Quote checking needs `pdftotext` (poppler), `pypdf`, or `pymupdf`, or pass `--text paper.txt` with text you extracted yourself.
5. Set `quiz: true` on the paper's entry, then commit and push in GitHub Desktop.

## Making a pitch video

With the `article-video` skill installed, attach the paper and ask Claude for a short video. Claude drafts a storyboard and script for your approval, renders a silent draft, and asks you to record the narration on your phone. It then cleans the recording and syncs the animation to your voice. Upload the result to YouTube and add its ID under `video` in the paper's entry. The skills were first written for Michael A. Heroux's papers, so they mention him by name, but they work the same for anyone. See [skills/article-video/SKILL.md](skills/article-video/SKILL.md).

## Installing the skills in the Claude app

For each skill: in Finder, right-click the skill's folder in `skills/` (for example `article-quiz`) and choose **Compress**. Then in the Claude app, go to **Settings > Capabilities > Skills** and upload the zip file.

## Previewing the site on your computer

```bash
bundle install
bundle exec jekyll serve   # http://localhost:4000/papers/
```

To test the quiz checker: `python3 -m unittest discover skills/article-quiz/tests`.

## License

Code is under the [MIT License](LICENSE). Documents, written content, images, audio, and video are under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/); see [LICENSE-CONTENT](LICENSE-CONTENT). © 2026 Michael A. Heroux.

In your own copy, keep the MIT notice for the code you copied, and add your name for your own work. For your content, choose any license you like and say so in `LICENSE-CONTENT`.

## Image policy

No photos or likenesses of any real person, living or dead, appear in this repo or on anything built from it. All images are created together with Claude, so Michael A. Heroux owns them and they are licensed CC BY 4.0. The only exception is Michael himself: his standard professional portrait and an occasional informal photo. Any other exception needs Michael's explicit approval for that specific case. Future contributors, including AI coding sessions, must follow this policy.
