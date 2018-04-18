---
layout: post
title:  "An Easy Way to Read and Write External Data in Android using FileProvider"
date:   2018-04-18 18:00:00
categories: Programming
tags: [java, android, programming]
---

One of the major issues I had while developing Arbor Familiae, my Android genealogy app, available at [https://github.com/sjmeunier/arbor-familiae](https://github.com/sjmeunier/arbor-familiae), is how to import and export data into, and out of the app. This, in itself, is not hard, but Android requires extra permissions to access external resources, which can be a big turn-off for users, so I searched for another way to do this. The solution is to use file providers, which is type of content provider.

The principle behind file providers, is that instead on the app reading and writing the data to external files directly, the app makes a request to an Intent to provide a data stream to either read or write the data to. This method allows the user to decide where the data goes to, while also not needing any special permissions in the app itself.

So, how does it all work?
<!--more-->

Firstly, we need to add a provider to the manifest file for the app, under the application tag.
Setting exported to false makes the provider only available for the app itself.
{% highlight xml %}
	<provider
		android:name="android.support.v4.content.FileProvider"
		android:authorities="com.sjmeunier.arborfamiliae.chartfileprovider"
		android:exported="false"
		android:grantUriPermissions="true">
		<meta-data
			android:name="android.support.FILE_PROVIDER_PATHS"
			android:resource="@xml/file_provider_paths"/>
	</provider>
{% endhighlight %}

Now, to import data into our application, we can create an intent, of type *Intent.ACTION_GET_CONTENT*. Once the launched activity returns, *onActivityResult* is called, where we can then get the Uri result from the intent.

The Uri may point either to an actual file or to a source that needs to be read via the content resolver, which is checked in the *readFile* method, and then an InputStream is opened from the Uri, which can be read, just like in normal file access.
If the file is obtained via a content provider, then provider needs to match the provider specified in the manifest for it to be able to resolve correctly.

{% highlight java %}
	private static final int FILE_SELECT_CODE = 0;

	private void showFileChooser() {
		Intent intent = new Intent(Intent.ACTION_GET_CONTENT);
		intent.setType("*/*");
		intent.addCategory(Intent.CATEGORY_OPENABLE);

		try {
			this.startActivityForResult(
					Intent.createChooser(intent, "Select a file to import"), FILE_SELECT_CODE);
		} catch (android.content.ActivityNotFoundException ex) {
			Toast.makeText(this, "Please install a File Manager.", Toast.LENGTH_SHORT).show();
		}
	}
	
	@Override
    public void onActivityResult(int requestCode, int resultCode, Intent data) {
        switch (requestCode) {
            case FILE_SELECT_CODE:
                if (resultCode == RESULT_OK) {
                    Uri uri = data.getData();
                    readFile(uri);
                }
                break;
        }
        super.onActivityResult(requestCode, resultCode, data);
    }
	
	private void readFile(Uri uri) {}
        InputStream inputStream;
        File file = null;
        if (uri.getScheme().equals("file")) {
            file = new File(uri.toString());
            inputStream = new FileInputStream(file);
        } else {
            inputStream = this.getContentResolver().openInputStream(uri);
        }
        BufferedReader br = new BufferedReader(new InputStreamReader(inputStream, "UTF-8"));

        String line;
        while ((line = br.readLine()) != null) {
            // do something with line from file
		}
		br.close();
	}
{% endhighlight %}

Writing to an external source is even easier. With a file we want to send to an external source, we first get a Uri for our file, which we are going to send to the intent. Then, we create a new intent with the type *Intent.ACTION_SEND*, setting the EXTRA_STREAM field to the uri. We also need to set the MIME type of the file, which in the example is text/plain. 

One little gotcha, is that when sharing via email, the Gmail application incorrectly set the from email as the uri path, so you need to explictly set the EXTRA_EMAIL parameter with an empty array to fix this. If you intend for the data to be sent by email and want to set a valid from address then you can add it here though, but this field will be ignored if the file is shared via other methods

Finally, we start the activity based on hte intent, which should give you an option of whatever apps are installed that can receive the data.
{% highlight java %}
	private static final int FILE_SELECT_CODE = 0;

	private void writeFile(File file) {
            Uri sharedFileUri = FileProvider.getUriForFile(this, "com.sjmeunier.arborfamiliae.fileprovider", file);
            Intent intent = new Intent(Intent.ACTION_SEND);
            intent.setType("text/plain");
            intent.setFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
            intent.putExtra(Intent.EXTRA_STREAM, sharedFileUri);
            intent.putExtra(Intent.EXTRA_EMAIL, new String[]{""});
            Intent chooser = Intent.createChooser("Choose app to send data to");
            if (intent.resolveActivity(this.getPackageManager()) != null) {
                this.startActivity(chooser);
            }
	}
	
{% endhighlight %}


More information on how to use file providers can be found in the [official documentation](https://developer.android.com/reference/android/support/v4/content/FileProvider.html)