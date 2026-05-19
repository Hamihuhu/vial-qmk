/*
 * Copyright (c) 2020 The ZMK Contributors
 *
 * SPDX-License-Identifier: MIT
 */

#include <behaviors.dtsi>
#include <dt-bindings/zmk/bt.h>
#include <dt-bindings/zmk/keys.h>
#include <dt-bindings/zmk/pointing.h>

&lt { quick-tap-ms = <180>; };

/ {
    behaviors {
        hrl: hrl {
            compatible = "zmk,behavior-hold-tap";
            label = "HRL";
            bindings = <&kp>, <&kp>;

            #binding-cells = <2>;
            tapping-term-ms = <250>;
            quick-tap-ms = <175>;
            require-prior-idle-ms = <150>;
            flavor = "balanced";
            hold-trigger-key-positions = <6 7 8 9 10 11 18 19 20 21 22 23 30 31 32 33 34 35 39 40 41>;
        };

        hrr: hrr {
            compatible = "zmk,behavior-hold-tap";
            label = "HRR";
            bindings = <&kp>, <&kp>;

            #binding-cells = <2>;
            tapping-term-ms = <250>;
            quick-tap-ms = <175>;
            require-prior-idle-ms = <150>;
            flavor = "balanced";
            hold-trigger-key-positions = <0 1 2 3 4 5 12 13 14 15 16 17 24 25 26 27 28 29 36 37 38>;
        };

        lt_alt: lt_alt {
            compatible = "zmk,behavior-hold-tap";
            label = "LT_ALT";
            bindings = <&mo>, <&kp>;

            #binding-cells = <2>;
            tapping-term-ms = <200>;
            quick-tap-ms = <180>;
            flavor = "tap-preferred";
        };
    };

    macros {
        shake_mouse: shake_mouse {
            compatible = "zmk,behavior-macro";
            #binding-cells = <0>;
            bindings =
                <&macro_press>,
                <&mmv MOVE_LEFT>,
                <&macro_release>,
                <&mmv MOVE_LEFT>,
                <&macro_press>,
                <&mmv MOVE_RIGHT>,
                <&macro_release>,
                <&mmv MOVE_RIGHT>;

            label = "SHAKE_MOUSE";
            wait-ms = <40>;
            tap-ms = <60>;
        };
    };

    keymap {
        compatible = "zmk,keymap";

        default_layer {
            display-name = "Default";

            // -----------------------------------------------------------------------------------------
            // |  TAB |  Q  |  W  |  E  |  R  |  T  |   |  Y  |  U   |  I  |  O  |  P  | BSPC |
            // | CTRL |  A  |  S  |  D  |  F  |  G  |   |  H  |  J   |  K  |  L  |  ;  |  '   |
            // | SHFT |  Z  |  X  |  C  |  V  |  B  |   |  N  |  M   |  ,  |  .  |  /  | ESC  |
            //                    | GUI | LWR | SPC |   | ENT | RSE  | ALT |

            bindings = <
&kp ESC           &kp Q            &kp W            &kp F         &kp P                &kp B          &kp J        &kp L                &kp U         &kp Y            &kp SEMI         &kp DELETE
&mt LEFT_GUI TAB  &hrl LEFT_GUI A  &hrl LEFT_ALT R  &hrl LCTRL S  &hrl LEFT_SHIFT T    &kp G          &kp M        &hrr LEFT_SHIFT N    &hrr LCTRL E  &hrr LEFT_ALT I  &hrr LEFT_GUI O  &kp APOS
&kp LSHFT         &kp Z            &kp X            &kp C         &kp D                &kp V          &kp K        &kp H                &kp COMMA     &kp DOT          &kp FSLH         &kp LEFT_SHIFT
                                                    &kp LCTRL     &lt_alt 1 BACKSPACE  &lt 6 ENTER    &lt 3 SPACE  &lt_alt 2 BACKSPACE  &mo 4
            >;
        };

        num_layer {
            display-name = "Number";

            // -----------------------------------------------------------------------------------------
            // |  TAB |  1  |  2  |  3  |  4  |  5  |   |  6  |  7  |  8  |  9  |  0  | BSPC |
            // | BTCLR| BT1 | BT2 | BT3 | BT4 | BT5 |   | LFT | DWN |  UP | RGT |     |      |
            // | SHFT |     |     |     |     |     |   |     |     |     |     |     |      |
            //                    | GUI |     | SPC |   | ENT |     | ALT |

            bindings = <
&trans    &kp FSLH  &kp N7  &kp N8  &kp N9  &kp MINUS    &trans  &trans     &trans     &trans    &trans    &trans
&kp BSPC  &kp N0    &kp N4  &kp N5  &kp N6  &kp PLUS     &trans  &kp LSHFT  &kp LCTRL  &kp LALT  &kp LGUI  &trans
&trans    &kp DOT   &kp N1  &kp N2  &kp N3  &kp EQUAL    &trans  &trans     &trans     &trans    &trans    &trans
                            &trans  &trans  &trans       &trans  &trans     &trans
            >;
        };

        sym_layer {
            bindings = <
&trans  &trans  &kp AMPS    &kp STAR     &kp LEFT_PARENTHESIS  &kp LEFT_BRACKET    &kp RBKT         &kp RIGHT_PARENTHESIS  &kp SLASH     &kp UNDERSCORE    &trans  &trans
&trans  &trans  &kp DOLLAR  &kp PERCENT  &kp CARET             &kp LEFT_BRACE      &kp RIGHT_BRACE  &kp PLUS               &kp EQUAL     &kp MINUS         &trans  &trans
&trans  &trans  &kp EXCL    &kp AT       &kp HASH              &kp BSLH            &kp PIPE         &kp LESS_THAN          &kp QUESTION  &kp GREATER_THAN  &trans  &trans
                            &trans       &trans                &trans              &trans           &trans                 &trans
            >;

            label = "Symbol";
        };

        nav_layer {
            bindings = <
&kp F1  &kp F2    &kp F3    &kp F4     &kp F5     &kp F6     &kp DOWN_ARROW  &kp RIGHT_ARROW  &trans  &trans  &trans  &trans
&trans  &kp LGUI  &kp LALT  &kp LCTRL  &kp LSHFT  &trans     &trans          &trans           &trans  &trans  &trans  &trans
&kp F7  &kp F8    &kp F9    &kp F10    &kp F11    &kp F12    &kp UP_ARROW    &kp LEFT_ARROW   &trans  &trans  &trans  &trans
                            &trans     &trans     &trans     &trans          &trans           &trans
            >;

            label = "Navigation";
        };

        media_layer {
            bindings = <
&trans  &trans  &trans  &trans  &trans  &trans    &kp C_VOL_DN  &kp C_PREV  &kp C_PLAY_PAUSE  &kp C_NEXT  &kp C_VOL_UP  &trans
&trans  &trans  &trans  &trans  &trans  &trans    &trans        &trans      &trans            &trans      &trans        &trans
&trans  &trans  &trans  &trans  &trans  &trans    &trans        &trans      &trans            &trans      &trans        &trans
                        &trans  &trans  &trans    &trans        &trans      &trans
            >;

            label = "Media";
        };

        test {
            bindings = <
&none  &kp Q  &kp W     &kp E  &kp R      &kp T        &kp Y  &none  &none  &none  &none  &tog 5
&none  &none  &kp LEFT  &none  &kp RIGHT  &none        &none  &none  &none  &none  &none  &none
&none  &none  &none     &none  &none      &none        &none  &none  &none  &none  &none  &none
                        &none  &none      &kp SPACE    &none  &none  &none
            >;

            label = "Test";
        };

        layer_7 {
            bindings = <
&trans  &trans  &kp LC(W)  &trans    &trans    &shake_mouse    &mmv MOVE_DOWN  &mmv MOVE_RIGHT  &trans        &trans           &trans  &trans
&trans  &trans  &mkp MB2   &mkp MB3  &mkp MB1  &kp LC(G)       &msc SCRL_LEFT  &msc SCRL_DOWN   &msc SCRL_UP  &msc SCRL_RIGHT  &trans  &trans
&trans  &trans  &trans     &trans    &trans    &trans          &mmv MOVE_UP    &mmv MOVE_LEFT   &trans        &trans           &trans  &trans
                           &trans    &trans    &trans          &shake_mouse    &trans           &trans
            >;
        };

        adjust_layer {
            bindings = <
&bt BT_CLR  &bt BT_SEL 0  &bt BT_SEL 1  &bt BT_SEL 2  &bt BT_SEL 3  &bt BT_SEL 4    &trans  &trans  &trans  &trans  &trans  &trans
&trans      &tog 5        &trans        &trans        &trans        &trans          &trans  &trans  &trans  &trans  &trans  &trans
&trans      &trans        &trans        &trans        &trans        &trans          &trans  &trans  &trans  &trans  &trans  &trans
                                        &trans        &trans        &trans          &trans  &trans  &trans
            >;

            label = "Adjust";
        };
    };

    conditional_layers {
        compatible = "zmk,conditional-layers";

        adj {
            if-layers = <1 2>;
            then-layer = <7>;
        };
    };
};
