#lv-architecture-interface-dom-sub

For context, thi architecture/interface was developed with the idea that there would be one main interface and there would be multiple other interfaces that would be "accessed" from that interface. I use subpanels because its easier to show what's going on, but this idea is also applicable if you hide/show front panels to the "in-focus" interface.

The expected use case for this is that all of the interfaces are NRE, the initial interface is the dom, while other interfaces are the sub. Although there's no problem with making the interfaces RE, you'll just need to modify

## Getting Started

Do the following to get started with this template/pattern:

 1. Copy the three libraries (lv-interface-dob-nre.lvlib, lv-interface-sub-common.lvlib, lv-interface-sub-re.lvlib) into a new folder.
 2. Duplicate the lv-interface-sub-re.lvlib for every sub interface you want to add.
 3. Modify lv-interface-sub-common.lvlib/interface-selection.ctl to include all of the interfaces you want to statically address.
 4. Modify lv-interface-dom-nre.lvlib/private/utilities/util_run_subs.vi to include all of the VIs you want to launch as sub interfaces (as coded, there's an assumption that they all have the same front panel).
 5. Run lv-interface-dom-nre.lvlib/main.vi, it should load the first interface and you should see the front panel updating.
 6. Click the Focus button to "start" updating its front panel.
 7. Click the UnFocus button to "stop" updating its front panel.
 8. Select a different interface and click the Swap button to change the front panel to the other interface.
 8. click the Focus/Unfocus as needed to understand what's going on in the background.
 9. Attempt to close the window to lv-interface-dom-nre.lvlib/main.vi to close the main and all other sub interfaces.

## Guidelines/Opinons

Below are some guidelines that can help provide guard rails when implementing this pattern/idea:

 * Although the example sub interface is re-entrant, it's still "static" by virtue of setting the interface type on startup and ensuring that it's unique. In the event you want to be able to communicate with re-entrant VIs that are not addressed statically, you'll need to add some kind of registration magic along with some way to uniquely identify each instance of the re-entrant VIs.
 * There's a lot of room for wasted CPU/effort, very clear usage of the focus/unfocus functions and undefer/defer panel updates can ensure that you don't waste cpu usage with an interface that's not currently visisble/in-use.
 * The arch always puts all of the interfaces into memory, if you want to "prevent" that you'll need to have some kind of dynamic launching ability.
 * Keep in mind that the interfaces, once launched STAY in memory, if you have a need to take them out of memory, you'll need to add that, but in that case you'll have less need for focus/un-focus. Take care to try to balance the efforts/reduction in user experience that may come with constantly loading and unloading an interface.
 * Although there's an explicit use of focus/un-focus and swap, the intention is for that to be associated with navigation; if I navigate to button X, it should "focus" the interface associated with X etc.
