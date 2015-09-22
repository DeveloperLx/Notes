# Notes
notes
----------------------------------------------------------------------------------

* ScrollView not scrolling when dragging on buttons

This is happening because UIButton subviews of the UIScrollView (I assume buttons are added as subviews in your case) are tracking the touches and not the scroll view. UIScrollView method touchesShouldCancelInContentView is the key here. According to its description: "The default returned value is YES if view is not a UIControl object; otherwise, it returns NO.", i.e. for UIControl objects (buttons), UIScrollView does not attempt to cancel touches which prevents scrolling.

So, to allow scrolling with buttons:

    Make sure UIScrollView property canCancelContentTouches is set to YES.
    Subclass UIScrollView and override touchesShouldCancelInContentView to return YES when content view object is a UIButton, like this:

- (BOOL)touchesShouldCancelInContentView:(UIView *)view
{
    if ( [view isKindOfClass:[UIButton class]] ) {
        return YES;
    }

    return [super touchesShouldCancelInContentView:view];
}

class UIButtonScrollView: UIScrollView {

    override func touchesShouldCancelInContentView(view: UIView!) -> Bool {
        if (view.isKindOfClass(UIButton)) {
            return true
        }

        return super.touchesShouldCancelInContentView(view)
    }
}
