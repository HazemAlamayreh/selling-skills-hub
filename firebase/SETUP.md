# External Training — Firebase integration checklist

Current GitHub Pages website: https://selling.idevelopmena.net
Firebase project (created by owner): **External Training**.
**IMPORTANT:** the website is not yet connected to Firebase; do not invite participants to share personal data until integration and security testing are complete.

## Firebase Console setup
1. Open https://console.firebase.google.com/ and select **External Training**.
2. Project settings > General > Your apps > Add app > Web (</>). Register e.g. `Selling Skills Web`. Copy the **firebaseConfig** object: `apiKey`, `authDomain`, `projectId`, `appId`, `messagingSenderId`, optional `storageBucket`.
3. Build > Authentication > Sign-in method: enable Email/Password (or chosen method). Add `selling.idevelopmena.net` to Authorized domains.
4. Build > Firestore Database > Create database, choose appropriate location and **production** security rules. Review the prepared `firestore.rules` before deploying.
5. Designate the owner's own Firebase Auth user as `kthAdmin: true` **via trusted Firebase Admin SDK**, never from browser JavaScript. Do not share service-account secrets.
6. Enroll participants through a trusted server path; public session links identify the cohort but must not independently confer database permission.
7. Implement verified assessment submission and scoring with trusted Cloud Functions / backend. Answers in downloadable HTML should not be treated as secure testing material.
8. Test with two participant accounts and the admin: participant A cannot see participant B, another cohort, or admin reports.
9. Confirm analytics collection notice, retention period, participant consent, export and deletion flows.

## Data path
- organizations/{organizationId}
- programs/{programId}
- sessions/{sessionId} (belongs to program and organization)
- participants/{uid} (linked to Firebase Authentication user)
- enrollments/{enrollmentId} (uid + sessionId)
- assessmentResponses/{responseId} (individual immutable attempts)
- assessmentResults/{resultId} (server-calculated)
- skillEvaluations/{evaluationId} (facilitator / administrator records)
- adminReports/{reportId} (private aggregate analytics)

## GitHub Pages
The existing HTML still saves user progress in browser localStorage. The admin dashboard CANNOT reliably aggregate these local values. Linking Firestore after configuration is a separate change. Use the exact Firebase Web App config (not a service-account key). GitHub Pages remains the public web host.

## Training access
Temporary trainer code is `hazem` (lowercase) and is only a convenience gate in public HTML. It does not provide secure authorization; migrate activation to Firestore session status controlled by admin before production.
