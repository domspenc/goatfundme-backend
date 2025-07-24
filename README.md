🐐 Project Idea: GoatFundMe (aka GtFndMe)
Yes, GoatFundMe. A hilariously niche crowdfunding platform where goats (and their owners) can raise money for utterly ridiculous, adorably chaotic, or heroically noble goat-related causes.

🐐 Concept Overview
This is a crowdfunding platform for goats. Not by goats (…yet), but for people who believe that goats deserve their dreams funded just like anyone else. Whether it’s:

New pajamas for winter

Therapy goat startup costs

A tiny goat bungee jumping rig

A romantic hay bale picnic for two

An aspiring goat influencer’s first GoPro

A goat named “Vincent Van Goat” who needs art supplies

If it involves goats, GoatFundMe makes it happen.

👥 Target Audience
Goat owners with delusions of grandeur.

Goat enthusiasts with internet access and no financial self-control.

Animal lovers who enjoy whimsical giving.

Vegans with a sense of humour.

Meme lords and Reddit weirdos.

🔐 User Accounts
Each user can:

Create goat-based crowdfunding projects.

Browse and pledge to others.

Comment support like “Heck yeah, Herbert needs that jetpack!”

Choose to remain anonymous if they’re pledging for “Goat Yoga for Goths”.

📦 Key Features (mapped to requirements)
Requirement	Feature
Django REST API	/goats, /pledges, /projects, /users endpoints
React Frontend	Fun, responsive goat-themed UI with ASCII goat error pages 🐐
User accounts	Username, email, password. Maybe a “Favourite Goat” field.
Create Project	Title: "Jetpack for Juniper", Description: "She's ready."
Pledge to Project	Amount, comment ("Fly, my queen."), anonymity toggle
Permissions	Users can only edit/delete their own goats… I mean projects
Custom error pages	"This goat got lost (404)" page
Token Auth	POST /api/token returns auth + goat's chosen spirit name 🧙
Responsive Design	Works beautifully on phones, tablets, and goat hooves (TBD)

🎨 Bonus Features (for kicks)
“Trending Goats” leaderboard

A badge system: e.g. “Gold Hoof Donor”, “Top Baaa-cker”

“Sponsor a Goat’s Dream” randomizer

Integration with goat-related GIF APIs for image uploads

Goat-themed loading animations: “Chewing grass…”

🧀 Name Variants (Missing Vowels Edition)
GtFndMe

BckThaGoat

GivAGot

GOTBUX

BLEATR

🌱 Why it works
Checks every technical box ✅

Appeals to a clear niche (weird internet + animal lovers) ✅

Makes your students actually want to build and test it ✅

Offers infinite comedy, creativity, and extensibility ✅

Uses real-world crowdfunding logic (Kickstarter-style) ✅

Sounds totally ridiculous — but they’ll remember it forever ✅



🧩 1. Wireframes (Structure Only, No Styling)
Here’s a rough breakdown of key pages and components. You can draw these up on draw.io, Figma, or just scribble on a napkin with goat hoofprints.

🏠 Home Page (/)
pgsql
Copy
Edit
+-------------------------------------------------+
| 🐐 GoatFundMe (Logo)       [Login] [Sign Up]    |
+-------------------------------------------------+
| 🔍 [Search Goats/Projects]                      |
| 🔥 Trending Projects | 🆕 New Projects          |
+-------------------------------------------------+
| Project Cards:                                  |
| [ 🖼️ Goat Image ]   "Jetpack for Juniper"       |
| Owner: @herbert_luvr   🟢 OPEN  💰 $350 / $900   |
+-------------------------------------------------+
🐐 Project Detail Page (/projects/:id)
pgsql
Copy
Edit
+-------------------------------------------------+
| < Back | 🐐 Jetpack for Juniper                 |
| Owner: @herbert_luvr     Created: 24 Jul 2025   |
+-------------------------------------------------+
| [ GOAT IMAGE 🖼️ ]                               |
| Description:                                    |
| "Juniper dreams of flight. She must ascend."    |
| Goal: $900    Raised: $350                      |
| Status: 🟢 Open for Pledges                     |
+-------------------------------------------------+
| 💸 [Make a Pledge]                              |
| - Amount: [ $___ ]                              |
| - Anonymous? [✔]                                |
| - Comment: [ "You go girl." ]                   |
| [ Submit Pledge ]                               |
+-------------------------------------------------+
| 💬 Pledges                                       |
| - @baahbud (anon): "$20 - you got this"         |
| - @hayqueen: "$50 - sky's the limit ✨"          |
+-------------------------------------------------+
🙋‍♀️ Create/Edit Project Page (/projects/new or /projects/edit/:id)
pgsql
Copy
Edit
+-------------------------------------------------+
| [Title]              "Goat Yoga for Goths"      |
| [Image Upload]       (drag or click)            |
| [Description]        textarea                   |
| [Target Amount]      $______                    |
| [Is Open?]           [✔] Accepting pledges      |
| [Submit / Update]                               |
+-------------------------------------------------+
🔑 Authentication Pages (/login, /signup)
Login

markdown
Copy
Edit
[ Email ]: ___________
[ Password ]: ________
[ Login Button ]
Signup

css
Copy
Edit
[ Username ]
[ Email ]
[ Password ]
[ Confirm Password ]
[ Signup Button ]
🧠 2. API Schema (DRF Models + Endpoints)
🔐 Auth
Method	Endpoint	Description
POST	/api/token/	Get token using username + password
GET	/api/user/	Return current user info (requires token)

👤 Users
DRF handles user creation via dj-rest-auth or custom views.

Field	Type
id	int
username	string
email	string
password	string

🐐 Projects
python
Copy
Edit
class Project(models.Model):
    title = models.CharField(max_length=200)
    owner = models.ForeignKey(User, on_delete=models.CASCADE)
    description = models.TextField()
    image = models.ImageField(upload_to="project_images/")
    goal_amount = models.DecimalField(max_digits=10, decimal_places=2)
    is_open = models.BooleanField(default=True)
    date_created = models.DateTimeField(auto_now_add=True)
Field	Type
id	int
title	string
owner	FK (User)
description	text
image	URL/path
goal_amount	decimal
is_open	boolean
date_created	datetime

💸 Pledges
python
Copy
Edit
class Pledge(models.Model):
    amount = models.DecimalField(max_digits=10, decimal_places=2)
    project = models.ForeignKey(Project, on_delete=models.CASCADE, related_name="pledges")
    supporter = models.ForeignKey(User, on_delete=models.CASCADE)
    anonymous = models.BooleanField(default=False)
    comment = models.TextField(blank=True)
    date_pledged = models.DateTimeField(auto_now_add=True)
Field	Type
id	int
amount	decimal
project	FK (Project)
supporter	FK (User)
anonymous	boolean
comment	string
date_pledged	datetime

📡 Endpoints Overview
Projects
Method	Endpoint	Description
GET	/projects/	List all projects
GET	/projects/:id/	Retrieve single project
POST	/projects/	Create project (auth required)
PUT/PATCH	/projects/:id/	Update project (owner only)
DELETE	/projects/:id/	Delete project (owner only)

Pledges
Method	Endpoint	Description
POST	/pledges/	Create pledge
GET	/pledges/	List pledges (staff/debug only)
DELETE	/pledges/:id/	Delete own pledge

🔐 Permissions
Only project owners can update/delete their projects

Only pledge creators can delete their pledges

Unauthenticated users can view projects, but not pledge or create

🧼 Error Handling
Custom 404 page on frontend:
“🧭 This goat wandered off. Try heading home.”

Backend:

Return proper 403, 401, 404, and validation errors

Token auth required for protected routes
