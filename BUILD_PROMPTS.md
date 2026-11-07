# Lift Frith Park Learning Platform: Phased Build Prompts

Paste ONE phase at a time into Codespaces. Wait until it works and you have tested it before moving on. Commit to Git after each phase:

```
git add . && git commit -m "Phase X done"
```

Every phase refers to `PROJECT_RULES.md`, so keep that file in the root of the repo.

---

## Phase 1: Project setup and database models

```
Follow PROJECT_RULES.md. PHASE 1: Foundation.

1. Inspect the workspace. Create a Flask project using the application factory pattern with this structure: app/ (blueprints for auth, main, student, teacher, admin), templates/, static/css, static/js, config.py, run.py, requirements.txt, .env.example, .gitignore, README.md.
2. Configure Flask-SQLAlchemy and Flask-Migrate to use a Supabase PostgreSQL connection string from DATABASE_URL. Add SECRET_KEY from env and secure session cookie settings for production.
3. Create these models: User (name, email, password_hash, role: student/teacher/admin, is_active), SchoolClass, Subject, Enrolment (student to class), TeachingAssignment (teacher to class and subject), Lesson, LessonCompletion, Assignment, Submission, Quiz, Question (four options, correct answer, explanation), QuizAttempt, Answer, Announcement. Include created_at/updated_at, foreign keys and sensible constraints.
4. Create the migration and a seed script that makes ONE admin account from env variables (no hard-coded passwords) plus an optional demo-data command.
5. Explain how to connect Supabase and run the migration step by step.

Do not build pages yet. Test that the app starts and tables are created.
```

---

## Phase 2: Branding, base layout and public home page

```
Follow PROJECT_RULES.md. PHASE 2: Design system and public pages.

1. Create base.html with Bootstrap 5, Bootstrap Icons, a custom CSS file with the navy/royal blue/teal theme, and a text-based logo (book or graduation-cap icon + "Lift Frith Park") placed in one reusable partial so it is easy to swap for an official logo later.
2. Create a responsive public navbar: Home, About, Learning Features, Contact, Login.
3. Build the home page: hero heading "Welcome to Lift Frith Park Learning Hub.", supporting text "Your learning journey, all in one place.", a prominent Student Login button, a short platform introduction, feature cards (Online Lessons, Assignments, Quizzes, Progress Tracking), a "How students use the platform" section, an FAQ accordion, and a footer with Privacy, Terms and Contact links.
4. Create simple About, Learning Features, Contact, Privacy and Terms pages with clearly marked placeholder text. Do not invent school contact details or policies.
5. Create flash message styling, and 404 and 500 error pages.

Make it mobile, tablet and desktop friendly. No private student data on public pages.
```

---

## Phase 3: Authentication and role protection

```
Follow PROJECT_RULES.md. PHASE 3: Authentication and roles.

1. Build login and logout with Flask-Login, Flask-WTF forms, CSRF protection, Werkzeug password hashing, validation and friendly error messages (do not reveal whether the email or the password was wrong).
2. No public registration. Accounts are created by admins only. Skip password reset unless an email service is configured; leave a clear placeholder.
3. Create role-based decorators (student_required, teacher_required, admin_required) and redirect each role to its own dashboard after login. Return 403 for wrong-role access.
4. Block inactive accounts from logging in. Validate the "next" redirect URL to prevent open redirects.
5. Build simple placeholder dashboards for each role with a responsive sidebar layout (collapses to a menu on mobile).
6. Write tests showing a student cannot open teacher/admin URLs.

Explain how the security parts work in simple words.
```

---

## Phase 4: Admin dashboard

```
Follow PROJECT_RULES.md. PHASE 4: Administrator features.

Build the protected admin area with:
1. Dashboard with real counts (students, teachers, classes, subjects, lessons, published announcements).
2. Create, edit, deactivate and list student and teacher accounts (admin sets a temporary password; hash it).
3. Manage classes and subjects.
4. Manage student enrolments in classes.
5. Assign teachers to classes and subjects.
6. Publish, edit and delete school-wide announcements.
7. View and manage all lessons and resources.

Use searchable, paginated tables, form validation, delete confirmations and flash messages. Only admins can reach these routes and only admins can assign roles.
```

---

## Phase 5: Subjects, lessons and student dashboard

```
Follow PROJECT_RULES.md. PHASE 5: Learning library.

TEACHER: create, edit, save as draft and publish lessons for classes/subjects they are assigned to. Fields: title, subject, class, topic, short description, written content, optional video URL, optional resource link, author, status, created/updated dates. Validate URLs (http/https only). No file uploads yet.

STUDENT: browse subjects for their enrolled classes, see only PUBLISHED lessons for their own classes, read lessons in a clean reader page (readable text, headings, previous/next navigation), and mark a lesson complete (stored in PostgreSQL, no duplicates).

STUDENT DASHBOARD: welcome message, enrolled classes and subjects, available lessons, completed lessons count, recent announcements, and quick-action buttons (Continue Learning, View My Subjects, My Assignments, Take a Quiz, View My Results). Everything comes from real data with friendly empty states.
```

---

## Phase 6: Assignments

```
Follow PROJECT_RULES.md. PHASE 6: Assignments.

TEACHER: create/edit assignments (title, instructions, class, subject, due date, maximum marks, draft or published, resubmission policy: allowed or not). Review submissions, assign marks (cannot exceed max), write feedback, and choose when to release marks/feedback.

STUDENT: view assignments for their classes, read instructions, submit a written answer, resubmit only if allowed, and see a clear confirmation. Statuses: Not started, Submitted, Late, Graded. Record submission timestamps and decide Late on the SERVER by comparing to the due date.

SECURITY: students can only view/edit their own submissions; marks and feedback are hidden until released; teachers only see submissions for their own classes. No file attachments yet.
Add tests for these permission rules.
```

---

## Phase 7: Quizzes

```
Follow PROJECT_RULES.md. PHASE 7: Quizzes.

TEACHER: create quizzes (class, subject, optional time limit, number of attempts allowed, publish/unpublish, whether students may see correct answers and explanations). Add multiple-choice questions with four options, one correct answer and an explanation.

STUDENT: list available quizzes, start an attempt, answer one question at a time with Next/Previous, see a progress indicator, a countdown for timed quizzes (JavaScript) and auto-submit when time ends. Untimed quizzes must also work.

SERVER RULES: calculate marks on the server only; never send correct answers to the browser during an attempt; enforce attempt limits and the time limit server-side; stop double-submits creating duplicate completed attempts; check the student is enrolled in that quiz's class. Show results, correct answers and explanations only if the teacher allows it.
```

---

## Phase 8: Progress, results and teacher tracking

```
Follow PROJECT_RULES.md. PHASE 8: Progress.

Build a student Progress page showing: subjects studied, lessons completed, assignments submitted, pending assignments, quiz attempts, quiz scores, average quiz performance and recent feedback. Use Bootstrap progress bars and one or two simple charts (Chart.js from a CDN is fine). Add a "My Results" page.

Add a teacher view of progress for students in the teacher's own classes only.

Everything must come from real database data with empty states for new accounts.
```

---

## Phase 9: Final polish, security review and deployment

```
Follow PROJECT_RULES.md. PHASE 9: Finish.

1. Review the whole app for security: CSRF on every form, permission checks on every route, no secrets in Git, secure cookies, safe redirects, input validation.
2. Check responsiveness and accessibility (labels, contrast, keyboard navigation).
3. Run all tests and fix failures.
4. Write a clear README: local setup, Supabase setup, env variables, migrations, creating the first admin, running tests.
5. Prepare deployment on a free-tier Python host (gunicorn, Procfile or equivalent, production env settings) with step-by-step instructions.
```

---

## Tips

- Never paste your Supabase password into the chat. Put it in the `.env` file.
- If something breaks, paste the exact error message and say "fix this without changing other features".
- If a phase prompt is still too long, split it at its numbered steps and send them one at a time.
- Possible later phases: secure file uploads, email password reset, admin reports.
