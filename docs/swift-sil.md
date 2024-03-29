# Swift Intermediate Language

> SIL is an SSA-form IR with high-level semantic information designed to implement the Swift programming language.

SIL is used for optimization, diagnostic, and distribute inline or generic code.

> A stable distribution format that can be used to distribute "fragile" inlineable or generic code with Swift library modules, to be optimized into client binaries.

> In contrast to LLVM IR, SIL is a generally target-independent format representation that can be used for code distribution, but it can also express target-specific concepts as well as LLVM can.

## Syntax

```
sil_stage canonical

import Swift

// Define types used by the SIL function.

struct Point {
  var x : Double
  var y : Double
}

class Button {
  func onClick()
  func onMouseDown()
  func onMouseUp()
}

// Declare a Swift function. The body is ignored by SIL.
func taxicabNorm(_ a:Point) -> Double {
  return a.x + a.y
}

// Define a SIL function.
// The name @_T5norms11taxicabNormfT1aV5norms5Point_Sd is the mangled name
// of the taxicabNorm Swift function.
sil @_T5norms11taxicabNormfT1aV5norms5Point_Sd : $(Point) -> Double {
bb0(%0 : $Point):
  // func Swift.+(Double, Double) -> Double
  %1 = function_ref @_Tsoi1pfTSdSd_Sd
  %2 = struct_extract %0 : $Point, #Point.x
  %3 = struct_extract %0 : $Point, #Point.y
  %4 = apply %1(%2, %3) : $(Double, Double) -> Double
  return %4 : Double
}

// Define a SIL vtable. This matches dynamically-dispatched method
// identifiers to their implementations for a known static class type.
sil_vtable Button {
  #Button.onClick: @_TC5norms6Button7onClickfS0_FT_T_
  #Button.onMouseDown: @_TC5norms6Button11onMouseDownfS0_FT_T_
  #Button.onMouseUp: @_TC5norms6Button9onMouseUpfS0_FT_T_
}
```
