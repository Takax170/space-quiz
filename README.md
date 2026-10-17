# Orbit | The space quiz

Orbit is a ten-question space quiz covering planets, astronomy, and space
exploration. Get instant answer feedback and explanations, then see your score,
accuracy, and a detailed answer review. You can filter the review to missed
questions, copy your result, and restart for another attempt.

The responsive interface includes light and dark themes, reduced-motion support,
and keyboard controls. Use A-D to choose an answer and Enter to continue after
answering. The built-in **Keyboard controls** guide explains navigation.

## Run locally

Clone the repository and open `index.html` directly in a browser:

```sh
git clone https://github.com/Takax170/space-quiz.git
cd space-quiz
open index.html # macOS
```

On other platforms, double-click `index.html` or use your browser's Open File
command. No server, dependencies, installation, or build step is required.
HTML, CSS, JavaScript, and decorative artwork are all contained in `index.html`;
the app makes no external resource requests. Quiz state is kept in memory and
resets when the page is reloaded.

If clipboard access is unavailable, **Copy result** provides a read-only summary
that you can select and copy manually.

## GitHub Pages

The website is published at <https://takax170.github.io/space-quiz/>.

The [Deploy GitHub Pages workflow](.github/workflows/deploy-pages.yml) prepares
and uploads a static-site artifact on pull requests to `main`. Pushes to `main`
also deploy that artifact to the `github-pages` environment. The workflow can
also be run manually from the Actions tab. Only `index.html` is published;
repository documentation and configuration are not included.

In **Settings > Pages > Build and deployment**, the source must be set to
**GitHub Actions**. Deployment uses GitHub's short-lived workflow token and
OpenID Connect; no repository secrets are needed.
