# swiftui_tiktok_video

```swift
//
//  ContentView.swift
//  TiktokPlayer
//
//  Created by paige shin on 2023/07/26.
//

import SwiftUI
import AVKit

struct ContentView: View {
    
    @State var index = 0
    @State var top = 0
    @State var data = [
        Video(id: 0, player: AVPlayer(url: URL(filePath: Bundle.main.path(forResource: "video1", ofType: "mp4")!)), replay: false),
        Video(id: 1, player: AVPlayer(url: URL(filePath: Bundle.main.path(forResource: "video2", ofType: "mp4")!)), replay: false),
        Video(id: 2, player: AVPlayer(url: URL(filePath: Bundle.main.path(forResource: "video3", ofType: "mp4")!)), replay: false),
        Video(id: 3, player: AVPlayer(url: URL(filePath: Bundle.main.path(forResource: "video4", ofType: "mp4")!)), replay: false),
    ]

    
    var body: some View {
        ZStack {
            
            PlayerScrollView(data: self.$data)
                
            
            VStack {
                
                HStack(spacing: 15) {
                    Button(action: {
                        self.index = 0
                    }) {
                        Text("Following")
                            .foregroundColor(self.top == 0 ? .white : Color.white.opacity(0.45))
                            .fontWeight(self.top == 0 ? .bold : .none)
                            .padding(.vertical)
                    }
                    
                    Button(action: {
                        self.top = 1
                    }) {
                        Text("For You")
                            .foregroundColor(self.top == 1 ? .white : Color.white.opacity(0.45))
                            .fontWeight(self.top == 1 ? .bold : .none)
                            .padding(.vertical)
                    }
                } //: HStack
                
                Spacer()
                
                HStack {
                    Spacer()
                    // MARK: Trailing TabBar
                    VStack(spacing: 35) {
                        Button {
                            
                        } label: {
                            Image(systemName: "photo")
                                .resizable()
                                .frame(width: 55, height: 55)
                                .clipShape(Circle())
                        }
                        
                        Button {
                             
                        } label: {
                            VStack(spacing: 8) {
                                Image(systemName: "suit.heart.fill")
                                    .font(.title)
                                    .foregroundColor(.white)
                                
                                Text("2.5k")
                                    .foregroundColor(.white)
                            } //: VStack
                        }
                        
                        Button {
                             
                        } label: {
                            VStack(spacing: 8) {
                                Image(systemName: "message.fill")
                                    .font(.title)
                                    .foregroundColor(.white)
                                
                                Text("1021")
                                    .foregroundColor(.white)
                            } //: VStack
                        }
                        
                        Button {
                             
                        } label: {
                            VStack(spacing: 8) {
                                Image(systemName: "arrowshape.turn.up.right.fill")
                                    .font(.title)
                                    .foregroundColor(.white)
                                
                                Text("Share")
                                    .foregroundColor(.white)
                            } //: VStack
                        }

                    } //: VStack
                    .padding(.bottom, 55)
                    .padding(.trailing)
                } //: HStack
                
                // MARK: TABBAR
                HStack {
                    Button {
                        self.index = 0
                    } label: {
                        Image(systemName: "house")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(self.index == 0 ? .white : Color.white.opacity(0.35))
                            .padding(.horizontal)
                    }
                    
                    Spacer(minLength: 0)
                    
                    Button {
                        self.index = 1
                    } label: {
                        Image(systemName: "magnifyingglass")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(self.index == 0 ? .white : Color.white.opacity(0.35))
                            .padding(.horizontal)
                    }
                    
                    Spacer(minLength: 0)
                    
                    Button {
                        self.index = 2
                    } label: {
                        Image(systemName: "plus")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(self.index == 0 ? .white : Color.white.opacity(0.35))
                            .padding(.horizontal)
                    }
                    
                    Spacer(minLength: 0)
                    
                    Button {
                        self.index = 3
                    } label: {
                        Image(systemName: "square.and.arrow.up.fill")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(self.index == 0 ? .white : Color.white.opacity(0.35))
                            .padding(.horizontal)
                    }
                    
                    Spacer(minLength: 0)
                    
                    Button {
                        self.index = 4
                    } label: {
                        Image(systemName: "person")
                            .resizable()
                            .frame(width: 25, height: 25)
                            .foregroundColor(self.index == 0 ? .white : Color.white.opacity(0.35))
                            .padding(.horizontal)
                    }

                } //: HStack
                .padding(.horizontal)
            } //: VStack
            // because we are ignoring safe area
            .padding(.top, UIApplication.shared.windows.first?.safeAreaInsets.top)
            .padding(.bottom, (UIApplication.shared.windows.first?.safeAreaInsets.bottom)! + 5)
            
        } //: ZStack
        .background(Color.black.edgesIgnoringSafeArea(.all))
        .edgesIgnoringSafeArea(.all)
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}


final class Host: UIHostingController<ContentView> {
    
    override var preferredStatusBarStyle: UIStatusBarStyle {
        return .lightContent
    }
    
}

struct Video: Identifiable {
    var id: Int
    var player: AVPlayer
    var replay: Bool
}

struct PlayerView: View {
    
    @Binding var data: [Video]
    
    var body: some View {
        VStack(spacing: 0) {
            ForEach(self.data.indices, id: \.self) { index in
                ZStack {
                
                    Player(player: self.data[index].player)
                        .frame(width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height)
                        .offset(y: -13)
                    
                    if self.data[index].replay {
                        Button(action: {
                            // playing the video again
                            self.data[index].replay = false
                            self.data[index].player.seek(to: .zero)
                            self.data[index].player.play()
                        }) {
                            Image(systemName: "goforward")
                                .resizable()
                                .frame(width: 55, height: 60)
                                .foregroundColor(.white)
                        }
                    }

                }
                
            }
            
        } //: VStack
        .onAppear {
            // doing it for first video because scrollview didnt drag yet...
            self.data[0].player.play()
            
            
            self.data[0].player.actionAtItemEnd = .none
            NotificationCenter.default.addObserver(forName: .AVPlayerItemDidPlayToEndTime, object: self.data[0].player.currentItem, queue: .main) { _  in
                // notifictation to identify at the end of the videos...
                
                // enabling replay button ...
                self.data[0].replay = true
            }
        }
    }
    
}

struct Player: UIViewControllerRepresentable {
    
    var player: AVPlayer
    
    func makeUIViewController(context: Context) -> AVPlayerViewController {
        let view = AVPlayerViewController()
        view.player = player
        view.showsPlaybackControls = false
        view.videoGravity = .resizeAspectFill
        return view
    }
    
    func updateUIViewController(_ uiViewController: AVPlayerViewController, context: Context) {
        
    }
    
}

struct PlayerScrollView: UIViewRepresentable {
    
    @Binding var data: [Video]
    
    func makeUIView(context: Context) -> UIScrollView {
        let view = UIScrollView()
        let childView = UIHostingController(rootView: PlayerView(data: self.$data))
        
        // each children occupies one full screen, so height = count * height of screen...
        childView.view.frame = CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height * CGFloat(self.data.count))
        // same here
        view.contentSize = CGSize(width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height * CGFloat(self.data.count))
        view.addSubview(childView.view)
        view.showsVerticalScrollIndicator = false
        view.showsHorizontalScrollIndicator = false
        
        // to ignore safe area
        view.contentInsetAdjustmentBehavior = .never
        view.isPagingEnabled = true
        view.delegate = context.coordinator
        
        return view
    }
    
    func updateUIView(_ uiView: UIScrollView, context: Context) {
        uiView.contentSize = CGSize(width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height * CGFloat(self.data.count))
        for i in 0..<uiView.subviews.count {
            uiView.subviews[i].frame = CGRect(x: 0, y: 0, width: UIScreen.main.bounds.width, height: UIScreen.main.bounds.height * CGFloat(self.data.count))
        }
    }
    
    func makeCoordinator() -> Coordinator {
        return Coordinator(parent: self)
    }
    
    class Coordinator: NSObject, UIScrollViewDelegate {
        
        var parent: PlayerScrollView
        var index = 0
        
        init(parent: PlayerScrollView) {
            self.parent = parent
        }
        
        func scrollViewDidEndDecelerating(_ scrollView: UIScrollView) {
            let currentIndex = Int(scrollView.contentOffset.y / UIScreen.main.bounds.height)
            if self.index != currentIndex {
                self.index = currentIndex
                for i in 0..<self.parent.data.count {
                    // pausing all other videos...
                    self.parent.data[i].player.seek(to: .zero)
                    self.parent.data[i].player.pause()
                }
                
                // playing next video ...
                self.parent.data[self.index].player.play()
                
                self.parent.data[self.index].player.actionAtItemEnd = .none
                NotificationCenter.default.addObserver(forName: .AVPlayerItemDidPlayToEndTime, object: self.parent.data[self.index].player.currentItem, queue: .main) { _  in
                    // notifictation to identify at the end of the videos...
                    
                    // enabling replay button ...
                    self.parent.data[self.index].replay = true
                }
                
            }
        }
        
    }
    
}

```
