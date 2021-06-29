---
id: c855cd15-a923-4f27-bfac-301954e7f7a5
title: Android
desc: ''
updated: 1624874627488
created: 1623246574987
---

# Why do I learn it?

- Not to be an expert on it. I learn it to better understand the difference between iOS and Android development so that I can understand it and communicate easily with my Android team mates.
- I can learn the tooling for Android CI/CD 

# Main learning repository:
- [faizmokhtar/android-learnings](https://github.com/faizmokhtar/android-learnings)
    - multiple toy projects to understand how to build Android apps

# Notes
- `Activity` is a term used for a page or a screen
- Activity Lifecycle
    - Every Activity has a parent Activity class that it extends
    - `override fun onCreate(savedInstanceState: Bundle?)`
        - The callback that we will used the most. Similar like iOS `viewDidLoad` 
        - Usually where we setup the UI of our Activity here by calling `setContentView` method
        - Only called once in its lifecycle unless the Activity is created again
    - `override fun onRestart()`
        - When Activity restarts, this is called immediately before `onStart`
    - `override fun onStart()`
        - When the Activity first comes into view
        - The first of the visible lifecycles methods
    - `override fun onRestoreInstanceState(savedInstanceState: Bundle?)`
        - If the state has been saved using `onSaveInstanceState(outState:Bundle?)`, it will called this method after `onStart()`
    - `override fun onResume()`
        - Run as the final stage of creating an Activity for the first time & also when the app has been backgrounded & then is brought into foreground
        - Upon completion, view is ready to be used
    - `override fun onSaveInstanceState(outState: Bundle?)`
        - to save the state of the activity
    - `override fun onPause()`
        - Called when Activity starts to be backgrounded or another dialog or Activity comes into the foreground
    - `override fun onStop()`
        - Called when Activity is hidden. Either by being backgrounded or another Activity is being launched on top
    - `override fun onDestroy()`
        - Called by the system to kill the Activity when system resources are low
        - Or when `finish()` is called explicitly on Activity
        - Or when Activity is being killed by the users themselves

# Useful links
- [How to store secret keys in Android](https://guides.codepath.com/android/Storing-Secret-Keys-in-Android)
- [Using ViewBinding to avoid findViewById dance](https://developer.android.com/topic/libraries/view-binding)
- 