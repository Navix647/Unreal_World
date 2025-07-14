# Skinning Decomposition
Skinning decomposition refers to the family of automated methods to convert any animated mesh sequences to skinning models and bone transformations. The power and usefulness of these methods in game production pipeline is any arbitrary, highly deformable models controlled by complex deformation rigs can be generated offline, and then converted to skinning model for real time replaying in game engines. This solution allows artists using all possible rigging tools without worrying about the performance penalty in the game play. Skinning decomposition can be used to compress animations of blendshapes, muscle simulation or cloth simulation. Independent components of the method can also be used to track bones animation or to solve skinning weights from range of motion (ROM) poses.

The state-of-the-art skinning decomposition solution was introduced in the SIGGRAPH Asia 2012 technical paper “Smooth Skinning Decomposition with Rigid Bones” by Binh Le and Zhigang Deng. Since then, this method has been implemented in some animation tools for game production, including Hans Godard's [skinning converter](https://lesterbanks.com/2015/04/skinning-converter-for-maya/), Autodesk Maya’s  [bake deformer](https://knowledge.autodesk.com/support/maya/learn-explore/caas/CloudHelp/cloudhelp/2018/ENU/Maya-CharacterAnimation/files/GUID-DD430C9B-95E7-4EBB-8D2B-A566018B4AC4-htm.html), or SideFX Houdini’s  [skinning converter](https://www.sidefx.com/tutorials/game-tools-skinning-converter/), to name a few. At EA, we also use this technique in our content pipeline quite successfully since 2015.

SEED is pleased to open source  _Dem Bones_, a library for skinning decomposition, implemented by Binh Le, the creator of the method. _Dem Bones_ offers a lot of refinements for better quality and faster performance compared to the original research paper.  _Dem Bones_ core library is C++ header-only solvers using  [Eigen](http://eigen.tuxfamily.org/)  and  [OpenMP](https://www.openmp.org/). We also include a minimalist command line tool for Windows and MacOS that can import and export data from standard asset formats:  [FBX](https://en.wikipedia.org/wiki/FBX)  and  [Alembic](https://en.wikipedia.org/wiki/Alembic_(computer_graphics)). The library and command line tool provide core functions of skinning decomposition such as auto generation of joints, solving sparse, non-negative, affine skinning weights and solving rigid joint transformations. The package is ready to use in research and prototyping, or it can be integrated into the production pipeline with minimum effort.

Check out Dem Bones at: [https://github.com/electronicarts/dem-bones](https://github.com/electronicarts/dem-bones)

Dem Bones is released under the  [BSD 3-Clause license](https://github.com/electronicarts/dem-bones/blob/master/LICENSE.md).


> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTkxMDI0OTQ2Nl19
-->