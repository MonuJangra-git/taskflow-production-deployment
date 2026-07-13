## 🖥️ Proof of Errors (Real Debugging Log)

These are actual terminal screenshots captured while deploying this project on a fresh Ubuntu VM — included to document the real troubleshooting process, not a sanitized "it just worked" writeup.

<details>
<summary><strong>1. DATABASE_URL not found during schema push</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/2e4d70da-53d9-430b-803c-c6fb4acd0f60" width="700">
</details>

<details>
<summary><strong>2. drizzle-kit: command not found</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/0c700f95-c7c7-4c19-9288-24d41530028b" width="700">
</details>

<details>
<summary><strong>3. TypeScript project reference build order failure</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/5dd875b9-90dd-4199-9ff0-0a10617072a8" width="700">
</details>

<details>
<summary><strong>4. Root build script failing on unrelated mockup-sandbox package</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/ea453b5d-054d-4302-a662-087f87430815" width="700">
</details>

<details>
<summary><strong>5. Vite build failing due to missing PORT env var</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/ad3c2e89-40cf-41c2-9e18-4179285b0f7b" width="700">
</details>

<details>
<summary><strong>6. systemd CHDIR permission denied</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/8da3a33d-23be-4b52-b54a-d252ab6e9759" width="700">
</details>

<details>
<summary><strong>7. EACCES in node_modules/.vite-temp after accidental sudo pnpm</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/6b78ea4e-68e5-4884-97b4-96bc0a0f33d9" width="700">
</details>

<details>
<summary><strong>8. Successful health check after all fixes ✅</strong></summary>
<br>
<img src="https://github.com/user-attachments/assets/287dcffa-6d29-4aff-8bff-18d381cce88a" width="700">
</details>

Each issue above is documented in detail with root cause and fix in the [Challenges Solved](#-challenges-solved) section.
> If you're reviewing this project: these screenshots exist because I'd rather show the real debugging process than pretend everything worked on the first try.
> Errors aren't proof of failure — they're proof of the debugging that actually happened.
> Every one of these errors cost time and frustration — but each one taught something that no tutorial could: how production systems actually break, and how to reason through fixing them.
