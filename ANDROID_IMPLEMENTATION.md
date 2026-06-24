# Android Dynamic Notification Implementation

This guide provides two ways to fetch the `notification.json` from GitHub in your Android application.

## 1. Using Retrofit (Java/Kotlin)

### Dependencies (build.gradle)
```gradle
dependencies {
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
}
```

### Data Model
```kotlin
data class NotificationResponse(
    val message: String
)
```

### API Interface
```kotlin
interface ApiService {
    @GET("notification.json")
    fun getNotification(): Call<NotificationResponse>
}
```

### Usage
```kotlin
val retrofit = Retrofit.Builder()
    .baseUrl("https://raw.githubusercontent.com/karyan779/starlink-bypass-notification/master/")
    .addConverterFactory(GsonConverterFactory.create())
    .build()

val apiService = retrofit.create(ApiService::class.java)
apiService.getNotification().enqueue(object : Callback<NotificationResponse> {
    override fun onResponse(call: Call<NotificationResponse>, response: Response<NotificationResponse>) {
        if (response.isSuccessful) {
            val message = response.body()?.message
            // Show this message in your Card/UI
        }
    }

    override fun onFailure(call: Call<NotificationResponse>, t: Throwable) {
        // Handle error
    }
})
```

---

## 2. Using Ktor (Kotlin Multiplatform/Modern Android)

### Dependencies (build.gradle)
```gradle
dependencies {
    implementation "io.ktor:ktor-client-core:2.3.0"
    implementation "io.ktor:ktor-client-android:2.3.0"
    implementation "io.ktor:ktor-client-content-negotiation:2.3.0"
    implementation "io.ktor:ktor-serialization-kotlinx-json:2.3.0"
}
```

### Usage
```kotlin
val client = HttpClient(Android) {
    install(ContentNegotiation) {
        json()
    }
}

suspend fun fetchNotification(): String? {
    val response: NotificationResponse = client.get("https://raw.githubusercontent.com/karyan779/starlink-bypass-notification/master/notification.json").body()
    return response.message
}
```

## JSON URL
The direct link to your notification file is:
`https://raw.githubusercontent.com/karyan779/starlink-bypass-notification/master/notification.json`
