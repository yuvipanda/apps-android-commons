**Archived**: Now lives in https://commons-app.github.io/

# The Wikimedia Commons Android App #

## Build Requirements ##

1. [Android SDK][1] (Level 15)
2. [Maven][2]

## Build Instructions ##

1. Set the environment variable `ANDROID_HOME` to be the path to your Android SDK
2. Run `mvn install` to build
3. Run `mvn android:deploy` to deploy to a device
4. There is no step 4

**Note**: Currently uses a bunch of dependencies that are staged at `yuvi.in/blog/maven`. Will be migrated to either [Maven Central][4] or a Wikimedia staging server soon.

## Set Up IntelliJ for Commons Android App Development ##

### Import and Compile Commons Android App ##

[Download IntelliJ][6]

1. Clone the repository.
2. Open IntelliJ.
3. Import Project:  
	File -> Import Project  
	or  
	Select 'Import Project' from the Quick Start menu  
4. Navigate to the folder with the cloned repository and press 'OK'.
5. Select 'Import Project from external model' -> 'Maven' and press 'Next'.
6. Make sure 'Search for projects recursively' and 'Import Maven projects automatically' are checked. Select 'Next'.
7. This section needs no modification. Select 'Next'.
8. This section needs no modification. Select 'Next'.
9. Make sure the 'Android SDK home path' points to the 'android-sdk' folder. If the dropdown next to 'Java SDK' is empty, hit the '+' button avobe the sidebar and select 'JDK'. Navigate to your jdk folder, select it, and hit 'OK'. Now select the newly added JDK and hit 'Next'.
10. This section needs no modifications. Select 'Next'.
11. Select 'Finish'.
12. After the program opens select 'Make project' - there should be errors.
13. Near the top of the file that is opened up, one of the offending lines should be "import android.support.v4.app.FragmentActivity;" - put your cursor on that line and hit 'alt'/'option'+'enter' to bring up the AutoFix dialog. Select the 'compatibility' option.
14. Select 'Make project' again. It should compile successfully.

## License ##

This software is licensed under the [Apache License][5].

## Bugs? ##

This software has no bugs. You can dispute this statement at [bugzilla][3]

## Code Structure ##

Key breakdowns:

Activities started within the UI:
* ContributionsActivity (ContributionsListFragment, MediaDetailPagerFragment, MediaDetailFragment) - main "my uploads" list and detail view
* LoginActivity - login screen when setting up an account
* SettingsActivity - settings screen
* AboutActivity - about screen

Activities receiving intents:
* ShareActivity (SingleUploadFragment, CategorizationFragment) - handles receiving a file from another app, accepting a title/desc, and slating it for upload
* MultipleShareActivity (MultipleUploadListFragment, CategorizationFragment) - handles receiving a batch of multiple files from another app, accepting a title/desc, and slating them for upload

Services:
* WikiAccountAuthenticatorService - authentication service
* UploadService - performs actual file uploads in background
* ContributionsSyncService - polls for updated contributions list from server
* ModificationsSyncService - pushes category additions up to server

Content providers:
* ContributionsContentProvider - private storage for local copy of user's contribution list
* ModificationsContentProvider - private storage for pending category and template modifications
* CategoryContentProvider - private storage for recently used categories


## On-Device Storage ##

Account credentials are encapsulated in an account provider. Currently only one Wikimedia Commons account is supported at a time. (Question: what is the actual storage for credentials?)

Preferences are stored in Android's SharedPreferences.

Information about past and pending uploads is stored in the Contributions content provider, which uses an SQLite database on the backend.

A list of recently-used categories is stored in the Categories content provider, which uses an SQLite database on the backend.

Captured files are not currently stored within the app, but are passed by content: or file: URI from other apps.

Thumbnail images are not currently cached. (?)






[1]: https://developer.android.com/sdk/index.html
[2]: https://maven.apache.org/
[3]: https://bugzilla.wikimedia.org/enter_bug.cgi?product=Commons%20App
[4]: http://search.maven.org/
[5]: https://www.apache.org/licenses/LICENSE-2.0
[6]: http://www.jetbrains.com/idea/download/index.html
