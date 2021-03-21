[![](https://jitpack.io/v/SoufianeKreX/Link-Previewer.svg)](https://jitpack.io/#SoufianeKreX/Link-Previewer)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

# Link Previewer

Link Previewer Library For Android built in Kotlin.
The library show a preview for any link like WhatsApp Chat Link Preview .

### I Welcome Anyone who can help add Improvement on this project.

---

## Gradle Dependency


##### Add this to app level build gradle file

~~~gradle
allprojects {
	repositories {
	...
	maven { url 'https://jitpack.io' }
	}
}
~~~

##### Add this to your module build gradle file

~~~gradle
dependencies {
	implementation "com.github.SoufianeKreX:Link-Previewer:1.0.1"
}
~~~

## The Usage

Add the view to xml Layout 

~~~xml
<com.soufianekre.urlpreviewer.views.UrlPreviewCard
	android:id="@+id/urlPreviewCard"					   
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>
~~~
Add the following code to your class

~~~kotlin
urlPreviewCard.setUrl(url,object : UrlPreviewCard.OnPreviewLoad {
            override fun onLinkLoaded(url:String,urlPreview: UrlPreviewItem) {
                Toast.makeText(context,
                    "The preview Is Working", Toast.LENGTH_LONG).show()
            }
        })
~~~
#### That is it. you can now see your Url Preview as CardView.

## Implementing Url Preview in RecyclerView 
  
It is Quite Simple just Your List item View in your xml Layout

##### list_item_url_preview.xml
~~~xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="10dp"
    android:layout_margin="10dp">

    <androidx.cardview.widget.CardView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="@dimen/spacing_small"
        android:clickable="true"
        android:orientation="vertical"
        android:padding="@dimen/spacing_small"
        app:cardElevation="@dimen/spacing_tiny"
        android:focusable="true">
        <TextView
            android:id="@+id/list_item_url_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="Url example : www.google.com"
            android:textSize="@dimen/font_normal"
            android:layout_margin="@dimen/spacing_small"
            />

    </androidx.cardview.widget.CardView>

    <com.soufianekre.urlpreviewer.views.UrlPreviewCard
        android:id="@+id/item_url_preview"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>

~~~
##### AdapterViewHolder.kt
~~~kotlin

class UrlPreviewViewHolder(itemView: View) : RecyclerView.ViewHolder(itemView){
    val url_text : TextView = itemView.findViewById(R.id.list_item_url_text)
    val urlPreviewView: UrlPreviewCard = itemView.findViewById<UrlPreviewCard>(R.id.item_url_preview)

    init {

    }
}
~~~

Make Your Adapter Implementation Looks Like this :

##### UrlPreviewAdapter.kt
~~~kotlin
class UrlPreviewAdapter(var context: Context, var urlList: ArrayList<String>) :
    RecyclerView.Adapter<UrlPreviewViewHolder>(),UrlPreviewCard.OnPreviewLoad {

    var webPreviews: HashMap<String, UrlPreviewItem> = HashMap()

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): UrlPreviewViewHolder {
        val root =
            LayoutInflater.from(context).inflate(R.layout.list_item_url_preview, parent, false)
        return UrlPreviewViewHolder(root)
    }

    override fun getItemCount(): Int = urlList.size

    override fun onBindViewHolder(holder: UrlPreviewViewHolder, position: Int) {
        val url = urlList[position]

        holder.urlPreviewView.clear()
        holder.url_text.text = url
        var linkToLoad : String? = null;
        // TODO : Check The link if it is valid

        linkToLoad = url;

        if (linkToLoad != null){
            if (!tryImmediatePreviewLoad(holder,url)){
                holder.urlPreviewView.setUrl(url,this);
            }
        }

    }

    override fun onLinkLoaded(url: String, urlPreview: UrlPreviewItem) {
        webPreviews[url] = urlPreview
    }

    private fun tryImmediatePreviewLoad(holder: UrlPreviewViewHolder, url: String): Boolean {
        return if (webPreviews.containsKey(url)) {
            holder.urlPreviewView.tag = url
            holder.urlPreviewView.displayPreview(webPreviews[url]!!)
            true
        } else {
            false
        }
    }
}
~~~

Check the sample App if you want to learn more about this part.

## License 
```xml
   Copyright 2020 soufianekremcht

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

     http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```
