
# **Questions:**
* Q.1  Write a test case to validate given function? *
```
fun add(int: a, int: b) {
  addUsingLibrary(a+1, b+1)
}
```
**Ans:**
```
package com.my.streamdata

import org.junit.Test
import org.mockito.Mockito.verify
import org.mockito.kotlin.argumentCaptor
import org.mockito.kotlin.mock



class SomeDependency {
    fun addUsingLibrary(someArgument: Int, someOtherArgument: Int) {
        // some code
    }
}

class SomeObject (private val someDependency: SomeDependency) {
    fun someFunction(a:Int,b:Int) {
        someDependency.addUsingLibrary(a+1, b+1)
    }
}

class SomeObjectTest {

    private val someDependency = mock<SomeDependency>()
    private val someObject = SomeObject(someDependency)

    @Test
    fun testSomeFunction(){
        // given
        val expectedProperty = 6

        // when
        someObject.someFunction(5,5)

        // then
        val argumentCaptor = argumentCaptor<Int>()
        verify(someDependency).addUsingLibrary(argumentCaptor.capture(), argumentCaptor.capture())
        assert(argumentCaptor.firstValue == expectedProperty)
        assert(argumentCaptor.secondValue == expectedProperty)

    }
}
```

**Q.2  Write a code print a stream A,B,C data in seuence i.e every time data should be A,B,C format?**

**Ans**

```
package com.my.streamdata

import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import androidx.lifecycle.ViewModelProvider
import io.reactivex.rxjava3.kotlin.Observables
import io.reactivex.rxjava3.kotlin.subscribeBy
import io.reactivex.rxjava3.kotlin.toObservable
import kotlinx.coroutines.Job

class StreamData : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val obs1 = listOf("on", "tw", "thre").toObservable()
        val obs2 = listOf("e", "o", "e").toObservable()
        val obs3 = listOf(1, 2, 3).toObservable()
        val zipped = Observables.zip(obs1, obs2, obs3) { o1, o2, o3 ->
            val output = "$o1$o2 = $o3"
            Log.i("OUTPUT", "output = " + output)
            output
        }

        zipped.subscribeBy(  // named arguments for lambda Subscribers
            onNext = { println(it) },
            onError =  { it.printStackTrace() },
            onComplete = { println("Done!") }
        )

    }


}

```

**Q.3  what is the difference between "min sdk , target sdk and compile with " ? in android**

**Ans**

**android:minSdkVersion**

An integer designating the minimum API Level required for the application to run. The Android system will prevent the user from installing the application if the system's API Level is lower than the value specified in this attribute. You should always declare this attribute.

**android:targetSdkVersion**

An integer designating the API Level that the application targets. If not set, the default value equals that given to minSdkVersion. This attribute informs the system that you have tested against the target version and the system should not enable any compatibility behaviors to maintain your app's forward-compatibility with the target version. The application is still able to run on older versions (down to minSdkVersion).

As Android evolves with each new version, some behaviors and even appearances might change. However, if the API level of the platform is higher than the version declared by your app's targetSdkVersion, the system may enable compatibility behaviors to ensure that your app continues to work the way you expect. You can disable such compatibility behaviors by specifying targetSdkVersion to match the API level of the platform on which it's running. For example, setting this value to "11" or higher allows the system to apply a new default theme (Holo) to your app when running on Android 3.0 or higher and also disables screen compatibility mode when running on larger screens (because support for API level 11 implicitly supports larger screens).

There are many compatibility behaviors that the system may enable based on the value you set for this attribute. Several of these behaviors are described by the corresponding platform versions in the Build.VERSION_CODES reference.

To maintain your application along with each Android release, you should increase the value of this attribute to match the latest API level, then thoroughly test your application on the corresponding platform version. Introduced in: API Level 4

**android:maxSdkVersion**

An integer designating the maximum API Level on which the application is designed to run. In Android 1.5, 1.6, 2.0, and 2.0.1, the system checks the value of this attribute when installing an application and when re-validating the application after a system update. In either case, if the application's maxSdkVersion attribute is lower than the API Level used by the system itself, then the system will not allow the application to be installed. In the case of re-validation after system update, this effectively removes your application from the device.

please go through this link for more details

http://developer.android.com/guide/topics/manifest/uses-sdk-element.html

