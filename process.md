# Process

## **PHASE 1: STRATEGY AND PLANNING**

1. **Define Objectives**

   * Showcase expertise in web development, brand identity design, and writing.
   * Attract potential clients or collaborators with compelling, detailed project pages.
   * Focus on creating a self-hosted, highly customizable solution (no reliance on a CMS).

2. **Content Inventory**

   * Gather detailed data for each project:
     * **Fields**: Web Development, UI/UX, Branding, Copywriting, etc.
     * **Roles**: Designer, Developer, Copywriter, etc.
     * **Problem, Solution, Success**: Breakdowns of each project.
     * **Creative Process**: Step-by-step insights.
     * **Deliverables**: Visuals, mockups, links.
   * Organize assets (images, videos, PDFs) and classify by project.

3. **Database Planning**

   * Use **MariaDB** for relational database. Schema design to reflect project details:
     * **Projects Table**:
       * `id`, `title`, `description`, `year`, `main_field`, `slug`, `created_at`, `updated_at`
     * **Creative Fields**:
       * `field_id`, `name`, `project_id`
     * **Roles**:
       * `role_id`, `name`, `project_id`
     * **Project Details**:
       * `problem`, `solution`, `process`, `success`, `deliverables`, `project_id`
     * **Media**:
       * `media_id`, `type` (image, video, etc.), `url`, `alt_text`, `project_id`
   * Normalize the schema to ensure efficient querying.

4. **Design Planning**

   * Stick with Vue.js for a modern, lightweight frontend.
   * Create wireframes for:
     * **Home Page**: Portfolio highlights, filters, and navigation.
     * **Project Pages**: Focus on storytelling with visuals and text.
     * **Filters/Search**: Dynamic filtering by field, role, and year.
     * **About Page**: bio, skills, and values.
     * **Contact Page**: Inquiry form integrated with Laravel backend.
   * Make the design mobile-first and accessible.

## **PHASE 2: TECH STACK CONFIRMATION**

1. **Frontend: Vue.js**

   * Framework: **Vue 3** with Composition API for better scalability.
   * State Management: **Pinia** (lightweight and Vue-native alternative to Vuex).
   * Routing: Use **Vue Router** for dynamic project pages and navigation.
   * Styling:
     * **Tailwind CSS** for rapid styling and consistent design.
     * Add **SCSS** for customizations if needed.

2. **Backend: Laravel**

   * Laravel offers robust features for API creation and database management.
   * Use **Eloquent ORM** to interact with MariaDB for querying and relationships.
   * RESTful API endpoints:
     * `GET /projects` Fetch all projects.
     * `GET /projects/:slug` Fetch details for a specific project.
     * `POST /contact` Handle form submissions.
   * Leverage Laravel's built-in tools:
     * **Sanctum** for authentication (if future admin panel is needed).
     * **Migrations** for database schema management.

3. **Database: MariaDB**

   * Optimize MariaDB for performance on VPS (set proper indexing for tables).
   * Create a robust backup schedule using Cloud Server Management or cron jobs.

4. **Hosting: VPS + Cloud Server Management**

   * Cloud Server Management simplifies server management:
     * Set up a LEMP stack (Linux, Nginx, MySQL, PHP).
     * Use Nginx for improved performance.
   * Set up **SSL certificates** with Let’s Encrypt via Cloud Server Management.
   * Deploy Laravel and Vue.js apps as separate services:
     * Serve the frontend app as a static site via Nginx.
     * Serve Laravel on a subdomain or API route (e.g., `api.yourdomain.com`).

## **PHASE 3: DEVELOPMENT PROCESS**

### **1. Database Setup**

* Create the database using MariaDB.
* Define the schema and relationships (as outlined in Phase 1).
* Populate sample data for testing during development.

### **2. Backend Development (Laravel)**

* Build RESTful APIs for:
  * Fetching projects (`GET /projects`).
  * Fetching project details by slug (`GET /projects/:slug`).
  * Handling form submissions (`POST /contact`).
* Use **Eloquent relationships** to link projects with creative fields, roles, and media.
* Add middleware for request validation (e.g., for contact form).
* Use Laravel's `resources` and `transformers` for consistent API responses.

### **3. Frontend Development (Vue.js)**

* Create components for:
  * **Home Page**: Display featured projects with filters.
  * **Project Pages**: Pull data dynamically using Vue Router and Laravel API.
  * **Filters/Search**: Add a filter bar with Vue's reactivity for dynamic updates.
* Fetch data from Laravel API using **Axios**.
* Use **Tailwind CSS** for responsive layouts and visual consistency.
* Implement animations (e.g., fade-ins, transitions) with **Vue Transition** or **GSAP**.

### **4. Media Handling**

* Store media files on the VPS ~~or integrate with **Cloudinary** (optional)~~.
* Create Laravel routes to serve optimized images for performance.

## **PHASE 4: FEATURES AND ENHANCEMENTS**

1. **Project Page Features**
   * Include:
     * **Gallery**: Swipeable carousel for images/videos.
     * **Detailed Sections**: Problem, Solution, Process, Success, Deliverables.
     * **Tech Used**: Display tools or technologies.
   * Use **Markdown** or a rich-text editor for flexible content updates.

2. **Search and Filters**
   * Use Vue's reactivity to filter projects by:
     * Creative fields, roles, year, or keywords.
   * Implement a **debounced search bar** for improved UX.

3. **Analytics and Monitoring**
   * Add Google Analytics or privacy-focused tools like **Plausible**.
   * Use Laravel's logging features to track API usage.

4. **Performance Optimization**
   * Enable server-side caching with **Laravel Cache** for API results.
   * Use **Vue Lazyload** for images to reduce initial load time.
   * Minify and bundle CSS/JS using tools like **Vite**.

5. **SEO Enhancements**
   * Generate dynamic meta tags for each project using Vue Router.
   * Use **Laravel Spatie's SEO Package** for backend-generated meta tags.

## **PHASE 5: TESTING AND DEPLOYMENT**

1. **Testing**
   * Test Vue.js frontend and Laravel backend integration locally and on staging.
   * Ensure API endpoints are fast and return correct data.
   * Check responsiveness on multiple devices and browsers.

2. **Deployment**
   * Use Cloud Server Management to deploy Laravel and Vue.js apps:
     * Configure Nginx to serve Vue static files.
     * Configure Laravel to run as a backend API.
   * Set up **environment variables** for database credentials, API keys, etc.
   * Use Git for version control and CI/CD pipelines (e.g., GitHub Actions) for seamless deployment.

## **PHASE 6: LONG-TERM MAINTENANCE**

1. **Content Updates**
   * Add new projects manually via Laravel database or admin tool (if created later).
2. **Enhancements**
   * Build an admin panel in Laravel for project management.
   * Add blog functionality to share insights and attract SEO traffic.
3. **Monitoring**
   * Set up alerts and logs for server performance via Cloud Server Management.
   * Monitor site speed and uptime with services like Pingdom or VPS Provider monitoring.

---
