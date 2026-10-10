# CommunityJobs

A community job posting and application website. People can browse local jobs, post their own, apply, and track application status, all from a single `index.html` file with no build step.

## Features

- **Accounts:** create an account or log in as a *Job Seeker* or an *Employer*; edit your details and change your password.
- **Find Jobs:** search and filter by category and job type; save jobs, comment on them, and share them.
- **Post a Job:** employers publish listings with title, company, category, type, location, salary, and description, and can delete their own postings.
- **Applications:** job seekers apply through a form and follow the status of each application; employers review applicants and set the status to *Pending*, *Under Review*, *Accepted*, or *Declined*.
- **Account dashboard:** your posted jobs, applications sent, applications received, and saved jobs in one place.

## Project files

| File | Purpose |
|------|---------|
| `index.html` | The whole website (HTML, CSS, and JavaScript in one file) |
| `communityjobs_supabase.sql` | Database schema for Supabase (accounts, jobs, applications, job transactions, security rules) |
| `README.md` | This file |

## Current status

The website currently saves its data in the visitor's own browser (`localStorage`). That means accounts, jobs, and applications are **not shared between people or devices** yet. The Supabase schema in this repository is ready, but `index.html` has not been connected to it. See *Connect to Supabase* below.

## Run it locally

No installation needed. Download the files and open `index.html` in a web browser.

## Publish on GitHub Pages

1. Put `index.html` at the top level of your GitHub repository.
2. In the repository, open **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select your main branch and the `/ (root)` folder, then save.
4. After a minute or two the site is live at `https://<your-username>.github.io/<repository-name>/`.
5. For a custom domain, enter it under **Settings → Pages → Custom domain** and add the DNS records GitHub shows you.

## Connect to Supabase

Supabase gives the site a shared online database, so accounts and jobs are saved for everyone.

1. Create a project at [supabase.com](https://supabase.com).
2. Open **SQL Editor → New query**, paste the contents of `communityjobs_supabase.sql`, and click **Run**. The file is safe to run again.
3. Copy your **Project URL** and your **publishable** (or `anon`) key from **Project Settings → Data API / API Keys**.
4. Add them to `index.html` when the Supabase connection code is added.
5. In **Authentication → URL Configuration**, set the **Site URL** to your GitHub Pages address or custom domain.
6. Decide whether new users must confirm their email (**Authentication → Providers → Email**).

> **Security:** only the publishable (or `anon`) key may appear in the website. Never put the `secret` or `service_role` key in this repository.

### What the database holds

| Table | What it stores |
|-------|----------------|
| `profiles` | Account details (name, email, role). Passwords are handled by Supabase Auth. |
| `jobs` | Job postings |
| `applications` | Who applied to which job, and the status |
| `saved_jobs` | Jobs a user has bookmarked |
| `job_comments` | Comments on jobs |
| `job_transactions` | A permanent activity log: job posted, job deleted, application submitted, status changed |

Row Level Security is switched on for every table: each person sees and changes only their own data, anyone can browse jobs and read comments, and the transaction log is read-only and filled in automatically.

## Notes

- Accounts created in the browser-only version cannot be moved into Supabase; those users register again.
- The sample jobs from the site are included in the SQL file and are only added when the `jobs` table is empty.
