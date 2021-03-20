# MuxLiveStreameriOS
A live streaming app for the Stonks iOS interview

**Specification Summary:** 
- Native Application (Xcode, Swift or Objective C)
- Stream from (mux.com) [https://docs.mux.com/guides/video/start-live-streaming], no broadcasting necessary. 
- Screen 1 (Home) 
  - keep live stream view fixed at top. 
  - Scrollable content below it. 
  - NavBar at the bottom should enable navigation between the two screens.  Or Tabbar? 
- Screen 2 
  - Scrollable content 
- Navigating
  - From Home -> 2 Live stream from player 1 should switch to mini mode, continuing to play the stream, bottom right.  
  - From 2 -> Home Live stream should return to fixed top view and continue streaming.
  - Smooth transition. 

**Additional notes**
- Should take 3 hours to a few days.
- Please make reasonable assumptions and design choices where you feel the specification is unclear or falls short. Reach out to us for any further clarifications. 
- *Testing the Stream* Larix Broadcaster can be used to test the stream. Create a new connection using the URL `rtmps://global-live.mux.com:443/app/<YOUR-STREAM-KEY>` and you will be good to go. Other broadcasting tools like OBS can also be used to get the job done.

**Deliverables**
- Screen recording of the application demonstrating navigation between the screens.
- Link to the public git repo containing your code along with a README file.

**Criteria**
1. Efficiency, performance, and readability of your code
2. Ability to navigate third-party libraries where the documentation might not be
very detailed.
3. Proper documentation (code comments and readme)\

# UI Plan #
The assignment includes a screen shot for each of the two required screens. 

The bottom has what appears as a tab view.  So this should have 3 or so viewControllers.  
1.  TabViewController. 
2.  HomeViewController.
3.  DetailsViewController.

## TabViewController ##
Since a smooth transition of the live stream View is desired I think might make sense to let the stream video view be a subView to the container view which is the TabViewController. 

So the TabViewController will have its default TabView at the bottom along with a stream video subView which would could initially be added as a subView to a fixed View in the HomeViewController.

At first I thought I would have to move the streamer view back and forth between the container and the contained views as the transition occurs.  However now I am thinking I only need to set the original constraints equal to a fixed view in the HomeViewController and animate them to the bottom right in the container without adding it as a subview to the contained views. 

**Transition Home->2** 
- *DidStartTransitioning*
  - animate the stream view to the bottom right of the view. and reduce its frame. 

**Transition 2->Home**
- *DidStartTransitioning*
  - animate the stream view from the bottom right of the view to the HomeViewController's fixed view's position and increase its frame to match it. 
    
    -  **Choices:** 
     - This can be done with constraints, or resizing the frame. Constraints are better practice. 
     - CoreAnimation directly, or UIView.animateWithDuration wrapper.  Try with UIView.animateWithDuration first, if that isn't catered properly, use CoreAnimation directly.  
     - Or reusing the wheel with a framework already made. Downside: might not move to the correct location.

Letting the Streaming player keep its same view allows us to avoid the problem of having to manage streaming state and position in the video which would open the door to more opportunities for errors and bugs.  

**viewControllers {set}**
- Append HomeViewController with title: "HOME"
- Append DetailsViewController with title: "SCREEN2"

## HomeViewController Plan ##
  `[ Label      ]
  [ view       ]
  [ TableView  ] 
    <Cell>[Label  ]
          [Content]`
  
## DetailsViewController Plan ## 
 ` [ Label     ]
  [ TableView ]
  <Cell>[ Label ]`
  

# Backend #
- Need to fetch the streaming content.  This will involve researching the following resources. 
  - [Mux](https://docs.mux.com/guides/video/start-live-streaming)
  - [AVPlayerLayer](https://developer.apple.com/documentation/avfoundation/avplayerlayer)
 
