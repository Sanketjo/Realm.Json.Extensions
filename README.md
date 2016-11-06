![RealmJson.Extensions](media/SushiHangover.RealmJson.png)

#SushiHangover.RealmJson.Extensions

####Extension Methods for adding JSON APIs to a Realm Instance 

##Current project status

| Git Branch | Xamarin.Android | Xamarin.iOS | Xamarin.Forms<sup>(1)</sup>
| :------ | :------: | :------: | :------: |
| **master** | ![](media/passing.png) | ![](media/passing.png) | ![](media/passing.png) |

<!--* The CI builds are generously hosted and run on ~~~
-->
##[Realm](https://realm.io/docs/xamarin/latest/) Versions


###Xamarin Realm v0.80.0

* Package Version `1.1.0`

	* Due to the changes in `Manage`, it is now faster to create/update `RealmObject`s when dealing with under 1k records then using `AutoMapper`, see [Perf.md](https://raw.githubusercontent.com/sushihangover/Realm.Json.Extensions/master/Perf.md) for details.
	* Replaced `AutoMapper` w/ `Manage` except for:
	* Added `CreateAllFromJsonViaAutoMapper`

###Xamarin Realm v0.78.1 

* Package Version `1.0.1`

	* Uses `AutoMapper` to auto-assign the Json derived object properies to the RealmObject
	* Initial Public Release

##Nuget

`PM> Install-Package RealmJson.Extensions`

Ref: [https://www.nuget.org/packages/RealmJson.Extensions](https://www.nuget.org/packages/RealmJson.Extensions)

##Issues?

###Post issues on [GitHub](https://github.com/sushihangover/Realm.Json.Extensions/issues)

##Need Help

Post on [StackOverflow](http://stackoverflow.com/questions/tagged/xamarin+realm) with the tags: **`[XAMARIN]`** **`[REALM]`** **`[JSON]`**


##Extension API

* A Realm Instance:
	* .CreateAllFromJson\<T\>(string)
	* .CreateAllFromJson\<T\>(Stream)
 	* .CreateAllFromJsonViaAutoMapper\<T\>(Stream)
	* .CreateObjectFromJson\<T\>(string)
	* .CreateObjectFromJson\<T\>(Stream)
	* .CreateOrUpdateObjectFromJson\<T\>(string)
 	* .CreateOrUpdateObjectFromJson\<T\>(Stream)


### <sup>(1)</sup>`Xamarin.Forms` Usage

`AutoMapper` does not support PCL `Profile 259`<sup>(2)</sup> and thus adding this Nuget package will fail if applied to a default `Xamarin.Form` project. 

You can change your Xamarin.From project to `Profile 
111`, then **retarget** the `Xamarin.Forms` package, and you will be able to add the Nuget package.

See the `Xamarin.Forms`-based (`Nuget.Test`) project in the code repo as an example.

Note: Once Xamarin has full support for `.NET Standard` and AutoMapper releases v5.2 <sup>(3)</sup>, this mess should go away.

<sup>(2)</sup> https://github.com/AutoMapper/AutoMapper/issues/1531

<sup>(3)</sup> https://github.com/AutoMapper/AutoMapper/issues/1532


##Usage / Examples
	
###Single RealmObjects from Json-based Strings:
	
	using (var theRealm = Realm.GetInstance(RealmDBTempPath()))
	{
		var realmObject = theRealm.CreateObjectFromJson<StateUnique>(jsonString);
	}

###Single RealmObjects from a Stream:

	using (var stream = new MemoryStream(byteArray))
	using (var theRealm = Realm.GetInstance(RealmDBTempPath()))
	{
		var testObject = theRealm.CreateObjectFromJson<StateUnique>(stream);
	}


###Json Arrays from a Android Asset Stream:

	using (var theRealm = Realm.GetInstance(RealmDBTempPath()))
	using (var assetStream = Application.Context.Assets.Open("States.json"))
	{
		theRealm.CreateAllFromJson<State>(assetStream);
	}

###Json Arrays iOS Bundled Resource Stream:

	using (var theRealm = Realm.GetInstance(RealmDBTempPath()))
	using (var fileStream = new FileStream("./Data/States.json", FileMode.Open, FileAccess.Read))
	{
		theRealm.CreateAllFromJson<State>(fileStream);
	}

**Note:** Consult the Unit Tests for more usage details

##Building

###`xbuild` or `msbuild` based:

	xbuild /p:SolutionDir=./ /target:Clean /p:Configuration=Release   RealmJson.Extensions/RealmJson.Extensions.csproj
	xbuild /p:SolutionDir=./ /target:Build /p:Configuration=Release RealmJson.Extensions/RealmJson.Extensions.csproj


##Testing / NUnitLite Automatation

###`Xamarin.Android`

	adb shell am instrument -w RealmJson.Test.Droid/app.tests.TestInstrumentation


Ref: [Automate Android Nunit Test](https://developer.xamarin.com/guides/android/troubleshooting/questions/automate-android-nunit-test/)

Ref: [Issue 32005](https://bugzilla.xamarin.com/show_bug.cgi?id=32005)


###`Xamarin.iOS`
	
	xbuild RealmJson.Test.iOS/RealmJson.Test.iOS.csproj /p:SolutionDir=~/
	xcrun simctl list
	open -a Simulator --args -CurrentDeviceUDID 360D3BC6-4A6D-4B7E-A899-5C7651EC2107
	xcrun simctl install booted  ./RealmJson.Test.iOS/bin/iPhoneSimulator/Debug/RealmJson.Test.iOS.app
	xcrun simctl launch booted com.sushihangover.realmjson-test-ios
	cat ~/Library/Logs/CoreSimulator/360D3BC6-4A6D-4B7E-A899-5C7651EC2107/system.log

##Performance Testing

##License

Consult [LICENSE](https://github.com/sushihangover/Realm.Json.Extensions/blob/master/LICENSE)