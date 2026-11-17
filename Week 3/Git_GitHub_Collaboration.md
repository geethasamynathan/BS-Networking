# Complete Git & GitHub Collaboration Tutorial

**Hands-on training:** Git, GitHub, VS Code, branching, pull requests, code reviews, and merge conflict resolution.

## Scenario and training environment

Build **SwiftBasket – Customer Order Tracking Portal**, a small HTML/CSS/JavaScript website. Simulate two developers using separate GitHub accounts and local VS Code folders.

| Role | GitHub username | Email |
|---|---|---|
| Owner | `geethasamynathan` | `geethasamynathan2011@gmail.com` |
| Collaborator | `geethasamynathan-lab` | `geetha.training@gamil.com` (as supplied; verify spelling) |

> **Email note:** The collaborator address was supplied with `gamil.com`. Confirm whether the verified account email is actually `geetha.training@gmail.com` before configuring the Git commit identity. You can invite the collaborator by username regardless of email.

| Property | Value |
|---|---|
| Repository | `swiftbasket-order-portal` |
| Project | SwiftBasket Customer Order Tracking Portal |
| Technology | HTML, CSS, JavaScript |
| Editor | Visual Studio Code |
| Main branch | `main` |
| Owner branch | `feature/customer-support` |
| Collaborator branch | `feature/order-tracking` |
| Merge conflict | Both developers modify the same HTML `<h1>` |

The final application has an order tracking form and a support section. No server, database or Node.js setup is required.

## 1. Git and GitHub fundamentals

- **Git:** Distributed version control system that records changes and supports independent development.
- **GitHub:** Repository hosting and collaboration platform providing issues, pull requests, reviews, permissions and automation.
- **Repository:** Project files and recorded history.
- **Clone:** Copy a remote repository to a local folder.
- **Working tree:** Local files being edited.
- **Stage:** Choose changes for the next commit.
- **Commit:** Record a snapshot locally.
- **Branch:** Independent line of development.
- **Push:** Upload local commits to a remote repository.
- **Fetch:** Download remote branch/commit information without integrating it.
- **Pull:** Fetch and integrate remote changes.
- **Pull request (PR):** Request to review and merge branch changes.
- **Merge:** Integrate one branch's changes into another.
- **Conflict:** Git cannot automatically reconcile competing changes.
- **`origin`:** Conventional name for the remote repository.
- **`HEAD`:** Reference to the currently checked-out commit.

### Typical team workflow

```text
GitHub Issue -> Create Feature Branch -> Edit Files -> Stage -> Commit
      -> Push Feature Branch -> Pull Request -> Review -> Merge to main
```

For a larger team:

| Developer | Task | Branch |
|---|---|---|
| 1 | Customer login | `feature/customer-login` |
| 2 | Product catalog | `feature/product-catalog` |
| 3 | Order tracking | `feature/order-tracking` |
| 4 | Payment | `feature/payment` |
| 5 | Tracking bug fix | `fix/tracking-validation` |

Developers clone the same repository and work on separate feature branches. A reviewed pull request integrates their changes into `main`.

## 2. Lab overview

1. Install Git and configure VS Code.
2. Create owner GitHub repository and initial website.
3. Invite the secondary account and clone the repository.
4. Assign issues and create two feature branches.
5. Implement each feature and push it.
6. Create, review and merge the owner's pull request.
7. Create a conflicting collaborator pull request and resolve it in VS Code.
8. Merge the collaborator PR, synchronize both workspaces, and inspect history.
9. Demonstrate common Git operations and troubleshooting.
10. Assign a student extension task.

---

# Lab 1 — Install Git and prepare VS Code

1. Install Git for Windows: https://git-scm.com/downloads
2. Install Visual Studio Code: https://code.visualstudio.com/download
3. Optionally install **GitHub Pull Requests and Issues** in VS Code.
4. In **VS Code > Terminal > New Terminal**, execute:

```powershell
git --version
git config --list
```

A typical Git version looks like `git version 2.xx.x.windows.1`.

### Understand commit identity versus authentication

- **Commit identity:** `user.name` and `user.email` are recorded in commits.
- **Authentication:** GitHub credentials determine whether a user is authorized to push.

Setting `user.email` does **not** change the authenticated GitHub account. This distinction is particularly important when using two GitHub accounts on one Windows PC.

---

# Lab 2 — Create the owner's repository and application

**Act as:** `geethasamynathan`.

## Step 1: Create the repository

Sign in at https://github.com, select **New repository**, and configure:

| Field | Value |
|---|---|
| Repository name | `swiftbasket-order-portal` |
| Description | `SwiftBasket customer order tracking application` |
| Visibility | Private |
| Initialize with README | Yes |
| .gitignore | None initially |

Expected repository URL (after creation):

```text
https://github.com/geethasamynathan/swiftbasket-order-portal
```

## Step 2: Clone into the owner folder

```powershell
mkdir C:\GitTraining\owner
cd C:\GitTraining\owner

git clone https://github.com/geethasamynathan/swiftbasket-order-portal.git
cd swiftbasket-order-portal
code .
```

Authenticate as the owner if prompted.

## Step 3: Configure repository-local owner identity

```powershell
git config --local user.name "geethasamynathan"
git config --local user.email "geethasamynathan2011@gmail.com"

git config user.name
git config user.email
git remote -v
git branch --show-current
```

The active branch should be `main`.

## Step 4: Create the following files

```text
swiftbasket-order-portal/
├── index.html
├── style.css
├── script.js
└── README.md
```

**`index.html`**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SwiftBasket Order Portal</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <header>
    <h1>SwiftBasket Order Portal</h1>
    <p>Track your orders quickly and easily.</p>
  </header>

  <main>
    <section>
      <h2>Welcome to SwiftBasket</h2>
      <p>Manage your online shopping orders.</p>
    </section>
  </main>

  <script src="script.js"></script>
</body>
</html>
```

The original `<h1>` will later create the controlled merge conflict.

**`style.css`**

```css
body {
  font-family: Arial, sans-serif;
  background: #f4f7fb;
  margin: 0;
  color: #243447;
}

header {
  background: #174ea6;
  color: white;
  padding: 32px;
  text-align: center;
}

main {
  max-width: 850px;
  margin: 30px auto;
}

section {
  background: white;
  padding: 25px;
  margin-bottom: 20px;
  border-radius: 10px;
  box-shadow: 0 2px 8px #00000010;
}

button {
  background: #174ea6;
  color: white;
  border: none;
  padding: 10px 16px;
  cursor: pointer;
}

input {
  padding: 10px;
}
```

**`script.js`**

```javascript
console.log("SwiftBasket application initialized");
```

**`README.md`**

```markdown
# SwiftBasket Order Portal

A collaborative GitHub training project.

## Technologies
- HTML
- CSS
- JavaScript

## Team
- Owner: geethasamynathan
- Developer: geethasamynathan-lab

## Features
- Customer order tracking (planned)
- Customer support (planned)

## How to Run
Open index.html in a web browser.
```

## Step 5: Commit and push the starter project

```powershell
git status
git add .
git commit -m "Initial SwiftBasket application setup"
git push origin main
```

| Command | Why it is used |
|---|---|
| `git status` | Inspect working tree changes |
| `git add .` | Stage changes in current directory |
| `git commit -m ...` | Record staged changes locally |
| `git push origin main` | Upload commits to the remote main branch |

Refresh the repository on GitHub to confirm that the new files appear.

## Alternative: VS Code Source Control UI

1. Press `Ctrl+Shift+G` to open **Source Control**.
2. Inspect the changed files and differences.
3. Click `+` to stage files.
4. Enter a commit message and select **Commit**.
5. Choose **Push** to upload the commit.

**Teaching point:** Save, stage, commit, and push are distinct operations.

---

# Lab 3 — Invite collaborator and clone into a second folder

## Owner: Invite collaborator

From the repository on GitHub:

1. Open **Settings**.
2. Navigate to **Collaborators** under access management.
3. Select **Add people**.
4. Search for `geethasamynathan-lab`.
5. Send the invitation.

In a separate browser profile, sign in as `geethasamynathan-lab` and accept the invitation via GitHub notifications or invitation email.

## Collaborator: Clone repository

```powershell
mkdir C:\GitTraining\collaborator
cd C:\GitTraining\collaborator

git clone https://github.com/geethasamynathan/swiftbasket-order-portal.git
cd swiftbasket-order-portal
code .
```

Authenticate with the collaborator account when prompted.

Configure its local Git identity. **Only use the following email if it is the verified account email**; replace it with the correct address otherwise.

```powershell
git config --local user.name "geethasamynathan-lab"
git config --local user.email "geetha.training@gmail.com"

git config user.name
git config user.email
git remote -v
```

### Keep the workspaces separate

```text
VS Code Window 1 (Owner)
C:\GitTraining\owner\swiftbasket-order-portal

VS Code Window 2 (Collaborator)
C:\GitTraining\collaborator\swiftbasket-order-portal
```

Separate browser profiles help distinguish GitHub sessions, **but do not necessarily separate Git credentials**. For reliable credential isolation, use separate Windows user accounts or independently configured SSH keys. Git Credential Manager may otherwise select the wrong account.

---

# Lab 4 — Create GitHub Issues and feature branches

## Issue #1: Customer Order Tracking

- **Assignee:** `geethasamynathan-lab`
- **Branch:** `feature/order-tracking`
- Add an Order ID input and Track Order button.
- Support demo IDs `SB1001` and `SB1002`.
- Show useful messages for unknown and empty IDs.
- Change the heading to `SwiftBasket Customer Order Tracking`.
- Test, commit, and push.

## Issue #2: Customer Support

- **Assignee:** `geethasamynathan`
- **Branch:** `feature/customer-support`
- Add a Customer Support section.
- Show demo support contact details and working hours.
- Change the heading to `SwiftBasket Customer Service Portal`.
- Test, commit, and push.

Create these in **GitHub > Issues > New issue**. If Issues are disabled, enable them in repository settings.

> **Critical ordering:** Both feature branches must be created from the original `main` before the owner's PR is merged. This ensures that both independently modify the same original heading and the later conflict can be reproduced.

## Collaborator: Create order tracking branch

```powershell
git switch main
git pull origin main
git switch -c feature/order-tracking
git branch --show-current
```

Expected: `feature/order-tracking`.

## Owner: Create customer support branch

```powershell
git switch main
git pull origin main
git switch -c feature/customer-support
git branch --show-current
```

Expected: `feature/customer-support`.

---

# Lab 5 — Implement features, commit, and push

## Part A: Collaborator implements order tracking

**Workspace:** `C:\GitTraining\collaborator\swiftbasket-order-portal`  
**Branch:** `feature/order-tracking`

### Edit `index.html`

Replace:

```html
<h1>SwiftBasket Order Portal</h1>
```

With:

```html
<h1>SwiftBasket Customer Order Tracking</h1>
```

Inside `<main>`, below the existing welcome section, add:

```html
<section id="tracking">
  <h2>Track Your Order</h2>
  <p>Enter your SwiftBasket order number.</p>

  <label for="orderId">Order ID</label>
  <input type="text"
         id="orderId"
         placeholder="Example: SB1001">

  <button type="button" onclick="trackOrder()">
    Track Order
  </button>

  <p id="trackingResult" role="status"></p>
</section>
```

### Replace `script.js`

```javascript
const orders = {
  SB1001: "Dispatched",
  SB1002: "Out for Delivery"
};

function trackOrder() {
  const orderId = document
    .getElementById("orderId")
    .value.trim().toUpperCase();

  const result = document
    .getElementById("trackingResult");

  if (!orderId) {
    result.textContent = "Please enter an Order ID.";
    return;
  }

  if (Object.hasOwn(orders, orderId)) {
    result.textContent =
      `Order ${orderId}: ${orders[orderId]}`;
  } else {
    result.textContent = "Order not found.";
  }
}
```

### Code explanation

- `orders` stores two demonstration order statuses.
- `getElementById("orderId")` selects the input element.
- `.trim()` removes leading/trailing whitespace.
- `.toUpperCase()` makes order IDs case-insensitive.
- `Object.hasOwn()` checks if the ID exists in the object.
- `textContent` safely displays the status.
- `return` stops processing when input is empty.

### Test in a browser

| Input | Expected output |
|---|---|
| `SB1001` | `Order SB1001: Dispatched` |
| `SB1002` | `Order SB1002: Out for Delivery` |
| `SB9999` | `Order not found.` |
| Empty | `Please enter an Order ID.` |
| `sb1001` | `Order SB1001: Dispatched` |

### Commit and push

```powershell
git status
git add index.html script.js
git commit -m "Add customer order tracking feature"
git push -u origin feature/order-tracking
```

`-u` configures upstream tracking for later plain `git push` commands.

**Checkpoint A:** Confirm `feature/order-tracking` exists on GitHub. **Do not merge it yet.**

## Part B: Owner implements customer support

**Workspace:** `C:\GitTraining\owner\swiftbasket-order-portal`  
**Branch:** `feature/customer-support`

Check:

```powershell
git branch --show-current
```

Replace the original heading in `index.html`:

```html
<h1>SwiftBasket Customer Service Portal</h1>
```

Add this section inside `<main>`:

```html
<section id="support">
  <h2>Customer Support</h2>

  <p>
    Our support team is available to assist
    you with orders and delivery questions.
  </p>

  <p>
    Support Email:
    <a href="mailto:support@example.com">
      support@example.com
    </a>
  </p>

  <p>Support Hours: Monday - Friday, 9 AM - 6 PM</p>
</section>
```

`support@example.com` is a placeholder, not an actual service desk address.

Open `index.html` in a browser and confirm the changed heading and support section appear.

Commit and push:

```powershell
git status
git add index.html
git commit -m "Add customer support section"
git push -u origin feature/customer-support
```

**Checkpoint B:** Both feature branches exist on GitHub, but neither is merged into `main`.

---

# Lab 6 — Pull requests and code review

## Step 1: Owner opens the support PR

As `geethasamynathan`, navigate to **Pull requests > New pull request**.

| Field | Value |
|---|---|
| Base | `main` |
| Compare | `feature/customer-support` |
| Title | `Add customer support functionality` |
| Description | Implements Issue #2 |

**Important:** The merge direction is `feature/customer-support` **into** `main`.

Suggested PR description:

```markdown
## Summary
Implemented the customer support feature for SwiftBasket.

## Changes
- Added Customer Support section.
- Added demonstration support contact information.
- Updated the page heading.

## Testing completed
- [x] Verified support section appears.
- [x] Verified page opens in the browser.
- [x] Checked HTML for missing closing tags.

Closes #2
```

Select **Create pull request**.

## Step 2: Review the PR

Show students the GitHub PR tabs:

| Tab | Purpose |
|---|---|
| Conversation | PR discussion and reviews |
| Commits | Commits in this PR |
| Checks | Automated tests/checks, if configured |
| Files changed | Line-by-line additions and deletions |

Demonstrate a review comment such as:

> Please verify that the support address is demonstrative and does not expose a personal email.

The owner can reply, update the feature branch, commit and push a correction. New commits appear automatically in the open PR.

## Step 3: Merge owner's PR first

After review, choose **Merge pull request > Confirm merge**. Prefer **Create a merge commit** for a visible classroom history if permitted by the repository settings.

Now `main` includes the support feature and its heading:

```html
<h1>SwiftBasket Customer Service Portal</h1>
```

The collaborator's independent branch still contains:

```html
<h1>SwiftBasket Customer Order Tracking</h1>
```

Do not delete the feature branch yet if you want to demonstrate the branch graph.

## Step 4: Collaborator opens order-tracking PR

As `geethasamynathan-lab`, choose **Pull requests > New pull request**.

| Field | Value |
|---|---|
| Base | `main` |
| Compare | `feature/order-tracking` |
| Title | `Implement customer order tracking` |
| Description | Implements Issue #1 |

Create the PR **without merging**. GitHub should flag that the shared heading cannot be automatically merged (provided the conflict setup has been followed exactly).

---

# Lab 7 — Reproduce and resolve a real merge conflict in VS Code

## Why the conflict occurs

Both feature branches were based on:

```html
<h1>SwiftBasket Order Portal</h1>
```

Owner changed it to:

```html
<h1>SwiftBasket Customer Service Portal</h1>
```

Collaborator changed it to:

```html
<h1>SwiftBasket Customer Order Tracking</h1>
```

Git can often merge changes in different files or different sections automatically. It cannot confidently decide between two incompatible edits to the **same line**.

## Step 1: Collaborator merges latest `main` into feature branch

In collaborator's VS Code terminal:

```powershell
git switch feature/order-tracking
git status
git fetch origin
git merge origin/main
```

Start from a clean working tree (`git status`).

Expected output resembles:

```text
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Run:

```powershell
git status
```

Expected relevant section:

```text
Unmerged paths:
  both modified: index.html
```

## Step 2: Inspect the conflict markers

Open `index.html`. Git may insert:

```html
<<<<<<< HEAD
<h1>SwiftBasket Customer Order Tracking</h1>
=======
<h1>SwiftBasket Customer Service Portal</h1>
>>>>>>> origin/main
```

| Marker | Meaning |
|---|---|
| `<<<<<<< HEAD` | Start of current branch's version |
| `=======` | Separator |
| `>>>>>>> origin/main` | End of incoming branch's version |

Because the merge was invoked while on `feature/order-tracking`, `HEAD` is the collaborator's feature branch and `origin/main` is the incoming source.

## Step 3: Resolve with VS Code Merge Editor

From **Source Control**, open the conflicted file. Depending on VS Code's UI/version, use the inline actions or **Open in Merge Editor** to compare current, incoming and result contents.

**Agreed final heading:**

```html
<h1>SwiftBasket Order Tracking & Customer Support</h1>
```

Replace the entire conflict block with that single line. Remove every conflict marker. Save.

**Business lesson:** Correct conflict resolution is not always “choose my version” or “choose their version.” Both features are required, so the team writes a new combined heading.

## Step 4: Finish the merge commit

```powershell
git add index.html
git status
git commit -m "Resolve heading conflict with customer support feature"
git push origin feature/order-tracking
```

The existing collaborator PR updates automatically when the branch is pushed.

## Step 5: Recheck the PR

On GitHub, refresh the collaborator PR. It should now be mergeable, assuming no subsequent changes or additional rules block it.

The owner reviews the final combined files and confirms:

- Order tracking input/button exist.
- JavaScript tracking works.
- Customer Support section exists.
- Heading reads **SwiftBasket Order Tracking & Customer Support**.
- No Git conflict markers remain.

## Step 6: Owner merges the collaborator PR

As owner, review **Files changed**, then select **Merge pull request > Confirm merge**, provided the repository's required checks and approvals are satisfied.

**Outcome:** `main` now includes both independently developed features.

---

# Lab 8 — Synchronize and verify the final application

## Step 1: Update owner workspace

```powershell
git switch main
git pull origin main
```

Open `index.html` locally. The final page should contain:

```text
SwiftBasket Order Tracking & Customer Support

Welcome to SwiftBasket
Manage your online shopping orders.

Track Your Order
Order ID: [ SB1001 ] [ Track Order ]
Order SB1001: Dispatched

Customer Support
Support Email: support@example.com
Support Hours: Monday - Friday, 9 AM - 6 PM
```

## Step 2: Inspect the Git history

```powershell
git log --oneline --graph --all --decorate
```

A simplified **illustrative** history (actual graph/hashes vary):

```text
*   Merge pull request: order tracking
|\
| *   Resolve heading conflict
| |\
| |/
| * Add customer order tracking feature
* | Merge pull request: customer support
|\|
| * Add customer support section
|/
* Initial SwiftBasket application setup
```

Inspect branches:

```powershell
git branch -a
```

This lists local and remote-tracking branches; deleted/pruned branches may no longer appear.

## Step 3: Update collaborator workspace

In the **collaborator's** terminal:

```powershell
git switch main
git pull origin main
```

Both local `main` branches now contain the merged application.

---

# Lab 9 — Additional real-world Git operations

## Essential command reference

| Command | Meaning |
|---|---|
| `git init` | Initialize local repository |
| `git clone URL` | Clone remote repository |
| `git status` | Show working tree state |
| `git add file` | Stage file |
| `git add .` | Stage changes under current directory |
| `git commit -m "message"` | Commit staged changes |
| `git branch` | Show local branches |
| `git branch -a` | Show local and remote-tracking branches |
| `git switch branch` | Change branches |
| `git switch -c branch` | Create/switch branch |
| `git push` | Push using upstream configuration |
| `git pull` | Fetch and integrate |
| `git fetch` | Download remote information |
| `git merge branch` | Merge branch into current branch |
| `git diff` | Show unstaged changes |
| `git log --oneline` | Concise commit history |
| `git stash` | Store uncommitted changes temporarily |
| `git stash pop` | Reapply and remove stash entry |
| `git merge --abort` | Attempt to cancel in-progress merge |
| `git revert HASH` | Create a new commit that reverses a commit |

## Non-fast-forward push rejection

If two developers push to the **same** shared branch, the remote may advance before the second developer pushes. Git may reject the stale push.

For a controlled shared-branch demo, while on `feature/shared-demo`:

```powershell
git fetch origin
git merge origin/feature/shared-demo
git push origin feature/shared-demo
```

Resolve any merge conflicts before completing the push. **Do not use `git push --force` as a default fix.**

## Abort a merge

If a merge is in progress and you want to cancel it before committing:

```powershell
git merge --abort
```

This is safest when the working tree was clean before the merge.

## Temporarily stash unfinished work

```powershell
git stash push -m "Temporary order tracking work"
git switch main
```

To return later:

```powershell
git switch feature/order-tracking
git stash pop
```

A stash is not a remote backup or normal commit.

## Merge strategies

| Strategy | Effect | Team usage |
|---|---|---|
| Merge commit | Preserves branch relationships in history | Teaching and collaborative history |
| Squash merge | Produces one combined commit on target | Concise main branch |
| Rebase merge | Replays commits in linear sequence | Linear history requirements |

---

# Lab 10 — Troubleshooting

### 1. `git` is not recognized

Install Git, restart VS Code, check the PATH environment variable, and verify `git --version`.

### 2. Push fails with 403 / permission denied

Check the GitHub account used for authentication, confirm collaborator invitation acceptance, and examine branch protections. `user.email` does not grant permissions.

### 3. `nothing to commit, working tree clean`

Check whether the files were saved, whether you are in the intended repository, and whether the change was already committed.

### 4. GitHub does not show your changes

Run:

```powershell
git status
git log -1 --oneline
git branch --show-current
git remote -v
```

Confirm that you **pushed** the correct branch.

### 5. PR shows no differences

Check the base/compare branch direction. The changes may already be merged or the wrong branch may have been selected.

### 6. The conflict does not appear

Verify that **both feature branches were created from the same original main** and changed the exact same heading before either feature was integrated. Ensure the collaborator branch was not previously synchronized with the updated main.

### 7. No conflict buttons in VS Code

Open Source Control, select the conflicted file, and use Merge Editor when available. You can also manually remove conflict markers and keep the correct final line.

### 8. `You have unmerged paths`

Resolve every conflicting file, stage it, and complete the merge commit; or use `git merge --abort` if abandoning the merge.

---

# Lab 11 — Production practices

In real teams, repository rules or branch protection can restrict direct pushes to `main` and enforce pull requests, reviewer approvals, or successful CI checks before merging. Availability depends on GitHub plan and repository type. Start the two-account classroom lab without mandatory approval requirements, then add protections for an advanced exercise.

A typical production process:

```text
Manager creates Issue
        |
Developer creates feature branch
        |
Developer codes, tests, commits and pushes
        |
Developer creates PR
        |
Reviewer inspects files and CI tests run
        |
Changes requested? -> Developer updates same PR
        |
Approved PR -> Merge to main
```

For advanced scenarios, show that branch protection may stop an author from approving their own PR or bypassing required checks.

---

# Student assignment — Order Cancellation Feature (25 marks)

**Branch:** `feature/order-cancellation`  
**Goal:** Add a small order cancellation feature to the SwiftBasket portal without editing `main` directly.

## Student requirements

- [ ] Create a GitHub issue and assign it to the developer.
- [ ] Create `feature/order-cancellation` from the current `main`.
- [ ] Add an Order ID input and Cancel Order button.
- [ ] Allow cancellation of demonstration order `SB1001`, but not `SB1002`.
- [ ] Show clear success and failure messages.
- [ ] Commit with an explanatory message and push the branch.
- [ ] Open a PR targeting `main` and document testing.
- [ ] Obtain review, address feedback, and merge after approval.

## Rubric

| Criterion | Marks |
|---|---:|
| Branch creation | 4 |
| Functional HTML and JavaScript | 8 |
| Commits and push | 4 |
| PR creation and description | 4 |
| Review, merge, verification | 5 |
| **Total** | **25** |

---

# Recap and interview questions

1. What is the difference between Git and GitHub?
2. Why do teams create feature branches?
3. What is the difference between saving, staging, committing and pushing?
4. Can Git merge two developers' edits to different files automatically?
5. When does a merge conflict occur?
6. Can you open a PR without immediately merging?
7. Why should `main` be protected?
8. How does `git fetch` differ from `git pull`?
9. When would you use `git merge --abort`?
10. How is GitHub push authentication different from commit author email?
11. How does GitHub's pull request compare with GitLab's merge request?
12. Why must the team test code again after a conflict resolution?

## Official documentation

- Git downloads: https://git-scm.com/downloads
- VS Code Git overview: https://code.visualstudio.com/docs/sourcecontrol/overview
- VS Code GitHub integration: https://code.visualstudio.com/docs/sourcecontrol/github
- Inviting GitHub collaborators: https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/inviting-collaborators-to-a-personal-repository
- Resolving merge conflicts: https://docs.github.com/en/pull-requests/how-tos/merge-and-close-pull-requests/resolving-a-merge-conflict-using-the-command-line

**Expected learning outcome:** Students can create repositories, collaborate through distinct GitHub accounts and feature branches, push code from VS Code, create/review pull requests, resolve merge conflicts, and verify a combined application on `main`.
