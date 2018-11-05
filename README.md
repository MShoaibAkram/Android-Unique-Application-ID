# Android-Unique-Application-ID

Step 1. Add it in your root build.gradle at the end of repositories:

	allprojects {
		repositories {
			...
			maven { url 'https://jitpack.io' }
		}
	}
Step 2. Add the dependency

	dependencies {
	        implementation 'com.github.MShoaibAkram:Android-Unique-Application-ID:Tag'
	}
  
Step 3. Call method Installation.id(Context context) to get unique identifier for your application. this identifier remains constant until or unless app installed again
