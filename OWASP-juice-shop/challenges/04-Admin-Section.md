## Challenge: Admin Section

### Objective
Access the administration section of the store.

### Solution
right click >> Inspect >> Ctrl+Shift+F >> searched for: `administration` >> I found: `path: 'administration'` >> altered url to: `http://localhost:3000/#/administration` >> SOLVED :)

### Relevant Concepts
* Broken Access Control
* Security through Obscurity

### Lessons Learned
Administrative interfaces should never rely on hidden or unlinked client-side routes for security. True access control must be strictly enforced on the server/API level, ensuring proper authentication and role checks regardless of whether a user discovers the frontend path.

---

## Coding Challenge: Related to Admin Section

### Objective
Click the line(s) containing the vulnerability.

### Solution
Select the route definition line or route guard configuration in the application routing file (`app.routing.ts` or similar) that exposes the administration path without enforcing administrative privileges or authentication guards.
<img width="735" height="350" alt="image" src="https://github.com/user-attachments/assets/3cda7802-6f18-4362-8147-293124cd965e" />


### Lessons Learned
Exposing administrative components in public frontend bundles without strict access guards and server-side authorization checks invites unauthorized access. Administrative utilities and routes must always be guarded on the client side and strictly validated on the backend.
<img width="1332" height="622" alt="image" src="https://github.com/user-attachments/assets/1d91d252-627b-4a59-b3aa-f280e5612280" />
