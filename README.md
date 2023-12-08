# first
//
//  BlurSelectorView.swift
//  CollageBuilder
//

import SwiftUI

struct BlurSelectorView: View {
    
    @Binding var blur: Blur
    
    @State private var blurType = BlurType.none
    
    @State private var boxRadius: CGFloat = 20
    @State private var discRadius: CGFloat = 20
    @State private var gaussianRadius: CGFloat = 20
    @State private var motionRadius: CGFloat = 20
    @State private var motionAngle: CGFloat = 0
    
    var body: some View {
        VStack {
            Picker("", selection: $blurType) {
                ForEach(BlurType.allCases, id: \.self) {
                    Text($0.rawValue)
                }
            }
            .pickerStyle(.segmented)
            slidersView
        }
        .onChange(of: blurType) { _ in handleChanges() }
        .onChange(of: boxRadius) { _ in handleChanges() }
        .onChange(of: discRadius) { _ in handleChanges() }
        .onChange(of: gaussianRadius) { _ in handleChanges() }
        .onChange(of: motionRadius) { _ in handleChanges() }
        .onChange(of: motionAngle) { _ in handleChanges() }
        .onAppear{ setupProperties() }
    }
    
    @ViewBuilder
    private var slidersView: some View {
        switch blurType {
        case .box:
            CommonSliderView(value: $boxRadius,
                             range: 0...100,
                             title: "Radius")
            
        case .disc:
            CommonSliderView(value: $discRadius,
                             range: 0...100,
                             title: "Radius")
            
        case .gaussian:
            CommonSliderView(value: $gaussianRadius,
                             range: 0...100,
                             title: "Radius")
            
        case .motion:
            CommonSliderView(value: $motionRadius,
                             range: 0...100,
                             title: "Radius")
            CommonSliderView(value: $motionAngle,
                             range: 0...180,
                             title: "Angle")
            
        case .none:
            EmptyView()
        }
    }
    
    private func handleChanges() {
        switch blurType {
            
        case .box:
            blur = .box(boxRadius
            )
        case .disc:
            blur = .disc(discRadius)
            
        case .gaussian:
            blur = .gaussian(gaussianRadius)
            
        case .motion:
            blur = .motion(motionRadius,
                           angle: motionAngle / 180 * .pi)
            
        case .none:
            blur = .none
        }
    }
    
    private func setupProperties() {
        switch blur {
        case .box(let radius):
            blurType = .box
            boxRadius = radius
            
        case .disc(let radius):
            blurType = .disc
            discRadius = radius
            
        case .gaussian(let radius):
            blurType = .gaussian
            gaussianRadius = radius
            
        case .motion(let radius, let angle):
            blurType = .motion
            motionRadius = radius
            motionAngle = angle * 180 / .pi
            
        case .none:
            blurType = .none
        }
    }
    
    private enum BlurType: String, CaseIterable {
        case box, disc, gaussian, motion, none
    }
}

struct BlurSelectorView_Previews: PreviewProvider {
    static var previews: some View {
        BlurSelectorView(blur: .constant(.gaussian(20)))
    }
}
