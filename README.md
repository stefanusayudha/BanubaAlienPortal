This is a project that launch single Banuba camera SDK. Specifically designed for the Anak Pintar Studio company needs.

# Launch the Activity via Unity
```objectivec
public class ActivityOverlayManager : MonoBehaviour
{
    private AndroidJavaObject currentActivity;

    void Start()
    {
        using (var unityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
        {
            currentActivity = unityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
        }

        // Start the native Android Activity
        using (var intent = new AndroidJavaObject("android.content.Intent", currentActivity, 
            new AndroidJavaClass("com.singularityindonesia.banuba_activity.BanubaActivity")))
        {
            currentActivity.Call("startActivity", intent);
        }
    }

    // Send intent to apply effect using broadcast.
    // Resend effect again to overrides the configuration.
    public void ApplyEffect(string effectConfig)
    {
        using (var intent = new AndroidJavaObject("android.content.Intent"))
        {
            intent.Call<AndroidJavaObject>("setAction", "com.singularityindonesia.banuba_activity.APPLY_EFFECT");
            intent.Call<AndroidJavaObject>("putExtra", "EFFECT", effectConfig);
            currentActivity.Call("sendBroadcast", intent);
        }
    }
}
```

## Effect Config Example:
```objectivec
const char* beautyConfig =
      "Lips.color(\"0.898 0.431 0.663 0.9\");\n"
      "Background.blur(0.8);";
```
