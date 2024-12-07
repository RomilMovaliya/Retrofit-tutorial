# Retrofit-tutorial
Retrofit is a type-safe HTTP client for Android and Java, specifically designed to simplify the process of consuming RESTful web services. It's a popular library developed by Square, known for creating other popular tools like OkHttp and Dagger.

# Key Features of Retrofit:
- `Type-safe HTTP API:` Define API endpoints using Java interfaces.   
- `Asynchronous Requests:` Easily make asynchronous network requests.   
- `Flexible Data Serialization:` Support for JSON, XML, and other formats.   
- `Interceptor Chain:` Customize network requests and responses.   
- `Error Handling:` Built-in error handling mechanisms.   
- `Built on OkHttp:` Leverages OkHttp's performance and features.

   

- Adding following Retrofit dependency in `app/build.gradle`.

```groovy

implementation'com.google.code.gson:gson:2.6.2'
implementation'com.squareup.retrofit2:retrofit:2.0.2'
implementation'com.squareup.retrofit2:converter-gson:2.0.2'
```

# Adding Internet permissions into our project
- In `/app/Manifests/AndroidManifest.xml`, adding the following permissions.
- In `AndroidManifest.xml` file,
  
```xml
<uses-permission android:name="android.permission.INTERNET" />
  ```

> `Note` : If your project has set `android.useAndroidX=true`, then you need to set `android.enableJetifier=true` in the `gradle.properties` file to migrate your project to AndroidX and avoid duplicate class conflict.

- In `MainActivity.java` file,
  
```java

    public String api="https://jsonplaceholder.typicode.com";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Retrofit retrofit=new Retrofit.Builder()
                .baseUrl(api)
                .addConverterFactory(GsonConverterFactory.create())
                .build();

        userInterface userInterface=retrofit.create(userInterface.class);

        //Network request
        Call<List<userModel>> call=userInterface.getUsers();

        call.enqueue(new Callback<List<userModel>>() {
            @Override
            public void onResponse(Call<List<userModel>> call, Response<List<userModel>> response) {

                if(response.isSuccessful()){
                    List<userModel> users= response.body();

                    for (userModel user : users) {
                        Log.d("User", user.getName());
                    }
                }else{
                    Log.e("Error", "Request failed with response code: " + response.code());
                }


            }

            @Override
            public void onFailure(Call<List<userModel>> call, Throwable t) {
                // Handle the failure
                Log.e("Error", "Request failed: " + t.getMessage());
            }
        });

    }
}
```

- In `userInterface.java` file,
```Java

package com.example.retrofitjava;

import java.util.List;

import retrofit2.Call;
import retrofit2.http.GET;

public interface userInterface {

    @GET("/users")
    Call<List<userModel>> getUsers();
}


```

- In `userModel.java` file,

```Java

package com.example.retrofitjava;

public class userModel {
    String id;
    String name;
    String username;
    String email;

    public userModel(String id, String name, String username, String email) {
        this.id = id;
        this.name = name;
        this.username = username;
        this.email = email;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}


```

