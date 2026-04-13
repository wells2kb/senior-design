## Test

To measure the performance of the plugin I created an intensive demo scene where two large Gaussian asset were constantly rotated.
This is an effective test because rotating the asset forces pixels to be drawn each frame and the distance buffer to be recalculated.

In my approach I used a global distance buffer over many individual asset distance buffers.
This could mean a performance loss, sorting many separate buffers is usually more faster than one big buffer due to time/space complexity and memory bottlenecks.
My approach also used one draw call instead of many separate draw calls which could mean a performance gain.

The measure for this test was the FPS as reported by Unity.

## Results

| before    | after     |
| --------- | --------- |
| 101±7 FPS | 97±10 FPS |

## Discussion

My test showed no significant change in performance.
This provided some key insights into what is actually slow and fast in the rendering pipeline.

The larger distance buffer most likely did not have a larger effect because a GPU radix sort is used.

Removing the multiple draw calls didn't have a larger effect most likely because draw calls are batched and the memory use is static.

Stepping into the Unity profiler showed that the largest contributor to the rendering time was the alpha blending.
This makes sense because that is something that didn't change and is notoriously slow.
