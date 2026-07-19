## 🖥️ Proof of Errors in Linux deployment (Real Debugging Log)

These are actual terminal screenshots captured while deploying this project on a fresh Ubuntu VM — included to document the real troubleshooting process, not a sanitized "it just worked" writeup.

<details>
<summary><strong>1. DATABASE_URL not found during schema push</strong></summary>
<br>
<img width="1316" height="521" alt="Screenshot 2026-07-13 235608" src="https://github.com/user-attachments/assets/98fcbe88-f7d3-4bfe-bdf8-12651a51551e" />

</details>

<details>
<summary><strong>2. drizzle-kit: command not found</strong></summary>
<br>
<img width="1252" height="397" alt="Screenshot 2026-07-13 235534" src="https://github.com/user-attachments/assets/2bb4f75b-0cfe-42c7-a164-9e48b071793b" /></details>


<details>
<summary><strong>3. TypeScript project reference build order failure</strong></summary>
<br>
<img width="1496" height="621" alt="Screenshot 2026-07-13 235411" src="https://github.com/user-attachments/assets/73d1292f-7bd3-46b5-95e5-ba5282c99781" /></details>

<details>
<summary><strong>4. Root build script failing on unrelated mockup-sandbox package</strong></summary>
<br>
<img width="1271" height="647" alt="Screenshot 2026-07-13 235553" src="https://github.com/user-attachments/assets/ccc64c07-517f-4be2-8d33-6a9f10b59eba" /></details>

<details>
<summary><strong>5. Vite build failing due to missing PORT env var</strong></summary>
<br>
<img width="1536" height="748" alt="Screenshot 2026-07-13 235404" src="..."/>
</details>

  
<details>
<summary><strong>6. systemd CHDIR permission denied</strong></summary>
<br>
<img width="970" height="586" alt="Screenshot 2026-07-13 235621" src="https://github.com/user-attachments/assets/d9696f03-0045-43ac-bcfc-27405885cf39" /></details>

<details>
<summary><strong>7. EACCES in node_modules/.vite-temp after accidental sudo pnpm</strong></summary>
<br>
<img width="1352" height="602" alt="Screenshot 2026-07-13 235643" src="https://github.com/user-attachments/assets/04c9767e-795b-4a3d-94f1-cef3a7b6a517" /></details>

## 🖥️ Proof of Errors in Linux deployment (Real Debugging Log)

<details>
<summary><strong>1. Missing environment variable "PORT" during Vite build</strong></summary>
<br>
  <img width="1044" height="440" alt="image" src="https://github.com/user-attachments/assets/1a51689e-ff61-40c1-a5ee-96263eac2f66" />

- **Category**: Configuration / Environment Error  
- **Root Cause**: The build process expects the `PORT` environment variable, but it wasn’t defined in `.env`, shell, or CI/CD pipeline.  
- **Common Scenario**: Happens when running `vite build` or `npm run dev` without setting required environment variables in `.env` or system environment.  
- **Fix**:  
  1. Add `PORT=3000` (or your desired port) in `.env` file.  
  2. Or export it directly in shell: `export PORT=3000` (Linux/macOS) / `set PORT=3000` (Windows).  
  3. Ensure CI/CD config (like GitHub Actions, Docker, or Vercel) passes the variable correctly.  
</details>
<details>
<summary><strong>2. MODULE_NOT_FOUND in Dockerized Node.js API</strong></summary>
<br>
<img width="757" height="657" alt="image" src="https://github.com/user-attachments/assets/c0354e3b-df5f-47e8-81ff-7e9e0ffc14e3" />
- **Category**: Dependency / Build Path Error  
- **Root Cause**: The container tried to run `/app/artifacts/api-server/dist/index.js`, but that file doesn’t exist inside the image. This usually happens when the build step didn’t generate the `dist` folder or the Dockerfile didn’t copy it correctly.  
- **Common Scenario**: Occurs when developers forget to run `npm run build` before building the Docker image, or misconfigure the Dockerfile COPY paths.  
- **Fix**:  
  1. Ensure `npm run build` (or equivalent) generates `dist/index.js`.  
  2. Update Dockerfile to copy the built files:  
     ```dockerfile
     COPY artifacts/api-server/dist ./artifacts/api-server/dist
     ```  
  3. Rebuild the image: `docker-compose build --no-cache`  
  4. Verify the file exists inside the container (`docker exec -it app_api ls /app/artifacts/api-server/dist`).  
</details>

<details>
<summary><strong>3. Docker Compose container restart loop and obsolete 'version' warning</strong></summary>
<br>
  <img width="565" height="276" alt="image" src="https://github.com/user-attachments/assets/2b3ccf31-9eea-41ba-964a-156a17b68a15" />

- **Category**: Configuration / Runtime Error  
- **Root Cause**:  
  1. The `version` key in `docker-compose.yml` is deprecated and ignored by newer Docker Compose versions.  
  2. The container restart loop indicates a failing service (likely due to misconfigured environment variables, missing dependencies, or incorrect health checks).  
- **Common Scenario**: Happens when older Compose files are used with newer Docker versions, or when a container crashes repeatedly during startup.  
- **Fix**:  
  1. Remove the `version:` line from `docker-compose.yml`.  
  2. Run `docker-compose logs <container_name>` to inspect the failing container’s error.  
  3. Verify service dependencies (e.g., database readiness, environment variables).  
  4. If persistent, rebuild containers:  
     ```bash
     docker-compose down
     docker-compose up --build
     ```  
</details>
<details>
<summary><strong>4. "Cannot GET /" on localhost:3000</strong></summary>
<br>
<img width="610" height="292" alt="image" src="https://github.com/user-attachments/assets/4fd1273d-9ee9-4daf-827c-d5dde3318fc9" />
- **Category**: Routing / Application Error  
- **Root Cause**: The server is running, but no route is defined for the root path (`/`). Vite/Express/Next.js apps often require explicit route handling or a default index page.  
- **Common Scenario**: Happens when backend or frontend frameworks don’t serve a default route, or when the build output isn’t correctly linked to the server entry point.  
- **Fix**:  
  1. Add a route handler for `/` in your server code (e.g., Express: `app.get('/', ...)`).  
  2. Ensure frontend build files are served correctly (check `vite.config.ts` or `next.config.js`).  
  3. If using React Router, add a `<Route path="/" element={<Home/>} />`.  
  4. Restart the server after changes.  
</details>
[Note:- During deployment with containers there are many error are caused due to some minimal code error and some due to ssl certifications are not used initially during testing.]

Each issue above is documented in detail with root cause and fix in the [Challenges Solved](#-challenges-solved) section.
> If you're reviewing this project: these screenshots exist because I'd rather show the real debugging process than pretend everything worked on the first try.
> Errors aren't proof of failure — they're proof of the debugging that actually happened.
> Every one of these errors cost time and frustration — but each one taught something that no tutorial could: how production systems actually break, and how to reason through fixing them.
