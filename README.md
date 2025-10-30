# RaggieSoft Web Monorepo

Welcome to the central development monorepo for all web properties created and maintained by Michael Ragsdale (raggiesoft). This repository contains the _source code_ for the entire RaggieSoft family of websites.

**Live Domains:**

- `raggiesoftknox.com`
    
- `raggiesoft.com`
    
- `michaelpragsdale.com`
    

---

## Core Philosophy: Source vs. Distribution

This project is built on a fundamental separation between **source code** (which lives in this Git repo) and **distribution code** (which is compiled and deployed).

1. **`raggiesoft-monorepo/` (This Repo)**
    
    - This is your "source of truth."
        
    - It **contains only** your development source files: `.scss`, `.php`, `.html`, and `.js` source.
        
    - This is the _only_ folder you should commit to Git.
        
2. **`raggiesoft-dist/` (The Output)**
    
    - This folder is **NOT** in Git. It is your build output.
        
    - The `npm` scripts in this repo will automatically compile/copy files from the monorepo _into_ this folder.
        
    - This folder is a clean, deployable mirror of what will be on your live servers.
        

---

## Repository Structure

```
raggiesoft-monorepo/
│
├── assets-src/           <-- All SCSS/JS source files go here
│   ├── common/           <-- (Shared across all sites)
│   │   └── scss/
│   │       └── bootstrap-overrides.scss
│   └── knox/             <-- (Project-specific)
│       └── scss/
│           └── theme-aerie-hold.scss
│
├── sites-php/            <-- All non-WordPress PHP/HTML sites
│   └── raggiesoftknox.com/
│       ├── www/
│       │   └── index.php
│       └── subdomains/
│           └── port/
│
├── wp-themes/            <-- Your custom WordPress themes
│   └── knox-lore/
│       ├── style.css
│       └── (other theme files...)
│
├── .gitignore            <-- Ignores 'node_modules'
│
└── package.json          <-- The "brain" of the build system
```

---

## Development Workflow (Getting Started)

This repository uses **NPM scripts** to automate all compiling and file management.

### Prerequisites

- [Node.js](https://nodejs.org/) (which includes `npm`)
    
- [rclone](https://rclone.org/) (for deploying assets)
    
- An SFTP client like [Filezilla](https://filezilla-project.org/) (for deploying sites)
    

### 1. Initial Setup

You only need to do this once on a new development machine.

Bash

```
# 1. Clone this repository
git clone [your-repo-url] raggiesoft-monorepo

# 2. Enter the repository
cd raggiesoft-monorepo

# 3. Install all development dependencies (sass, cpy-cli, etc.)
npm install
```

### 2. Day-to-Day Development

This is the command you will run 99% of the time.

Bash

```
# Run the master "watch" command
npm run watch
```

This single command starts multiple parallel processes that "watch" your source folders:

- When you save a `.scss` file in `assets-src/`, it is instantly compiled, minified, and saved to `../raggiesoft-dist/digital-ocean/`.
    
- When you save a `.php` file in `sites-php/`, it is instantly copied to `../raggiesoft-dist/glowing-galaxy/`.
    

### 3. One-Time Build

If you just want to compile everything once without watching, run:

Bash

```
npm run build
```

---

## Deployment Process

Deployment is a **manual, two-part sync** from your `raggiesoft-dist` (output) folder. **You do not `git pull` on the production server.**

### Part 1: Deploy Static Assets (CSS, JS, Images)

The `npm run build` or `npm run watch` commands place all compiled assets in `raggiesoft-dist/digital-ocean/`. You sync this folder to your DigitalOcean Space using `rclone`.

Bash

```
# Sync the 'digital-ocean' output folder to your Space
rclone sync C:\raggiesoft-website\raggiesoft-dist\digital-ocean do-spaces:assets.raggiesoft.com
```

### Part 2: Deploy PHP Sites

The build scripts copy your PHP sites into `raggiesoft-dist/glowing-galaxy/`. You sync this folder to your server's `/var/www` directory using an SFTP client.

- **Local Source:** `C:\raggiesoft-website\raggiesoft-dist\glowing-galaxy\`
    
- **Remote Target:** `/var/www/`
    

### Part 3: Deploy WordPress Themes

WordPress themes are _not_ part of the build process. You must deploy them manually.

- **Local Source:** `raggiesoft-monorepo/wp-themes/knox-lore/`
    
- **Remote Target:** `/var/www/raggiesoftknox.com/subdomains/lore/wp-content/themes/knox-lore/`
    

---

## Projects in this Monorepo

### The RaggieSoft Family

This repository contains the source code for all sites, including:

- **raggiesoftknox.com:** A narrative-focused hub site.
    
- **raggiesoft.com:** The main "RaggieSoft" brand website.
    
- **https://www.google.com/url?sa=E&source=gmail&q=michaelpragsdale.com:** The professional portfolio website.
    

### Featured Project: Knox Narrative Universe

**The Premise** On the oppressive, high-gravity jungle world of **Telsus Minor**, the monopolistic **Axiom corporation** rules through economic force. Their operations are plagued by a phantom saboteur they call **"Knox"**—a myth they believe to be a single, highly-trained operative.

The Axiom hunts a ghost of their own making. The reality is far more dangerous: **Anya and Kael Rostova**, young fraternal twins raised in "The Weave," wage a secret war using scavenged tech, improvised chemistry, and an intimate knowledge of the lethal environment the Axiom dismisses as the "Green Hell."

**Knox Project Architecture**

- **`https://raggiesoftknox.com/` (Main Site):** The primary hub, built with custom PHP from the `sites-php/raggiesoftknox.com/www/` folder.
    
- **`https://lore.raggiesoftknox.com/` (The Lore Bible):** A WordPress site. Its custom theme is located in `wp-themes/knox-lore/`.
    
- **`https://pact.raggiesoftknox.com/` & `https://port.raggiesoftknox.com/` (Narrative Sites):** Custom PHP sites from the `sites-php/` folder.
    

---

## Technology Stack

- **Frontend:** Custom PHP, Self-Hosted WordPress, HTML5
    
- **Styling:** SCSS, Bootstrap 5, WP Dark Mode Ultimate
    
- **Build Tools:** NPM Scripts, **Sass**, **cpy-cli**
    
- **Server:** DigitalOcean Droplet (`glowing-galaxy`) running Ubuntu 25.04
    
- **Web Server:** Nginx, PHP 8.4
    
- **Database:** MariaDB
    
- **Asset Hosting:** DigitalOcean Spaces (via `assets.raggiesoft.com`)
    
- **CDN & Security:** Cloudflare (DNS, WAF), UFW, Fail2Ban, SSH Key Authentication
    
- **Version Control:** Git, GitHub
    
- **Deployment:** `rclone` (for assets), SFTP (for site code)
    

---

## Triple-Licensing

This repository contains three distinct types of intellectual property, governed by separate licenses:

1. **Narrative Content (CC BY-SA 4.0):**
    
    - **Applies to:** All story text, character profiles, world-building descriptions, and lore entries.
        
    - **License:** Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0).
        
    - **Permissions:** You are free to share and adapt this creative work, even commercially, provided you give appropriate credit to Michael Ragsdale (raggiesoft), indicate if changes were made, and distribute your contributions under the same license.
        
2. **Website Source Code (MIT):**
    
    - **Applies to:** All code inside the `sites-php/` folder. This includes general PHP, JavaScript, Nginx configurations, and deployment scripts.
        
    - **License:** The MIT License (See `LICENSE.md`).
        
    - **Permissions:** You are free to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the software.
        
3. **WordPress Theme Code (GPL v3):**
    
    - **Applies to:** All code inside the `wp-themes/` folder. As a derivative work of WordPress, these themes are licensed under the GPL.
        
    - **License:** GNU General Public License v3.0.
        
    - **Permissions:** You are free to run, study, share, and modify the software. Any distributed derivative works must also be licensed under the GPL.
        

---

## Contributing

This is currently a personal project, but guidelines may be added later for community contributions to the lore.

_This README reflects the project state and architecture as of October 30, 2025._