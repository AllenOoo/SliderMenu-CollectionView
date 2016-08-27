# SliderMenu-CollectionView
滑动菜单和CollectionView的组合


SliderMenu 的简单使用

        let sliderMenu = SliderMenuView.init(frame: CGRectMake(0, 20, self.view.frame.width, 44))
        // 数据
        sliderMenu.titles = ["你","好","Swift","编程","之","美","新生","梦","想"]
        // 点击事件代理
        sliderMenu.delegate = self
                
                protocol SliderMenuViewDelegate {
                      func didSelectedAtIndex(index: NSInteger , animated: Bool)
                }

        // 设置初始位置
        sliderMenu.index = 3
        
CollectionView 滑动时的代理

        protocol SliderMenuInfoViewDelegate {
                /// 手指还未离开屏幕时的移动
                func collectionView(collectionView: SliderMenuInfoView, panning pan: UIPanGestureRecognizer)
                /// 手指离开屏幕时，targetIndex : 将要停止的到目标Cell的 Index
                func collectionView(collectionView: SliderMenuInfoView, didEndPan pan: UIPanGestureRecognizer, to targetIndex: NSInteger)
        }


SliderMenu＋CollectionView 联动设置

        sliderMenu.delegate = self
        //MARK: - SliderMenuViewDelegate
            func didSelectedAtIndex(index: NSInteger, animated: Bool) {
                // 点击时使下方collectionView滚动
                collection.scrollToItemAtIndexPath(NSIndexPath.init(forRow: index, inSection: 0), atScrollPosition: .CenteredHorizontally, animated: animated)
            }
            
        collection.gestureDelegate = self
        //MARK: - SliderMenuInfoViewDelegate
            func collectionView(collectionView: SliderMenuInfoView, panning pan: UIPanGestureRecognizer) {
                // 手指移动时改变滑动菜单对应Item缩放系数
                sliderMenu.itemScaledByDistance(Double(pan.translationInView(collectionView).x))
            }

            func collectionView(collectionView: SliderMenuInfoView, didEndPan pan: UIPanGestureRecognizer, to targetIndex: NSInteger) {
                // 手指离开屏幕后设定滑动菜单对应Item最终状态
                sliderMenu.sliderMenuWillScrollToIndex(targetIndex)
            }

