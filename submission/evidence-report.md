# FestPass CI Pipeline — Project Evidence Report

## 1. Project Overview

**Project:** FestPass — Campus Event Registration  
**CI/CD Tool:** Jenkins  
**Source Control:** Git  
**Runtime:** Java  
**Jenkins Job:** `festpass-ci`

The purpose of this project was to configure a Jenkins pipeline that checks out the FestPass source code, runs automated tests, and packages the application when the tests pass.

## 2. Initial Failed Build (Red Build)

- **Jenkins job:** `festpass-ci`
- **Build number:** 1
- **Commit ID:** a2ce1e00d874dc19683748481d8210d7ca9eaa39
- **Result:** Failed

The initial build exposed a cancellation-related defect in the booking service. The cancellation logic checked whether a booking existed but did not remove the student from the event’s student list.

The failed pipeline recorded diagnostic evidence in `submission-evidence.csv`. Packaging and subsequent verification stages were skipped because the tests failed.

## 3. Bug Fix

I updated `BookingService.java` so that cancelling a valid booking also removes the student ID from the event’s student list.

This fixes the cancellation behavior and allows the event’s seat to be reused after cancellation.

**Fix commit:** `f5cbc61` — `Fix booking cancellation and seat reuse`

## 4. Successful Build (Green Build)

- **Jenkins job:** `festpass-ci`
- **Build number:** 2
- **Commit ID:** f5cbc61d70fbc6fbbdf9b58cf32e68182c608eb2
- **Result:** Success

After the fix, the local verification reported **28 tests and 0 failures**. The application JAR was built successfully as `build/festpass.jar`, and all seven smoke-test command checks passed when run locally against the JAR.

The successful Jenkins build’s `submission-evidence.csv` and packaged JAR were saved for submission.

## 5. Downloaded JAR Verification

I downloaded the JAR from the successful Jenkins build and ran:

`java -jar .\festpass.jar help`

The application displayed its name, usage instructions, available commands, default data file, and documented exit codes. The command completed with exit code `0`, confirming that the downloaded JAR starts and displays help successfully.

**Screenshot:** `jar-help.png`

## 6. Submission Evidence

The submission includes:

- `red/submission-evidence.csv` — evidence from the failed build.
- `green/submission-evidence.csv` — evidence from the successful build.
- `green/festpass.jar` — packaged application from the successful build.
- `jar-help.png` — screenshot of the downloaded JAR’s help output.

## 7. Conclusion

The cancellation defect was corrected, local automated verification passed with 28 tests and no failures, and the downloaded application JAR