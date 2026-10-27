# Changelog

All notable changes to `tron2-gazebo-ros` will be documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Open-source scaffolding: `NOTICE`, `THIRD_PARTY_NOTICES.md`,
  `SECURITY.md`, `CONTRIBUTING.md`, `CHANGELOG.md`.
- GitHub CI workflow: `xmllint` on every `package.xml`, bundled-binary
  deny-list (outside `limxsdk-sim/lib/`), EXIF scan on `doc/` media,
  `<license>` value guard (fails CI when the value is `TODO` or
  `Proprietary`), private-IP scan with an allowlist for the known
  placeholder in `limxsdk-sim/include/limxsdk/apibase.h`.
- Issue templates and PR template under `.github/`.
- `.github/CODEOWNERS` with `maintainers`, `legal`, `sdk`,
  `robotics`, and `content` teams (placeholders — replace with real
  GitHub handles before enabling branch protection).
- `README.md`: SPDX header, "License & attribution",
  "Scope / not included", "Verification" (`catkin_make` + `xmllint`),
  and "Cite & support" sections.
- `package.xml` (both packages): `<url>` (website / repository /
  bugtracker), `<author>`, and enriched `<maintainer>` metadata.
- `.gitignore`: C++/CMake build artifacts, ROS 1/2 build dirs,
  editor / OS junk, and a runtime deny-list for artifacts that must
  not be committed here (`*.onnx`, `*.pt`, `*.pth`, `*.ckpt`,
  `*.bag`, `*.mcap`, and — outside `limxsdk-sim/lib/` — `*.so`,
  `*.dll`, `*.dylib`, `*.lib`, `*.whl`).

### Fixed
- `limxsdk-sim/CMakeLists.txt` no longer copies its headers into
  `devel/include`; its older `limxsdk/datatypes.h` shadowed
  limxsdk-lowlevel's and broke `tron2_hw` in the same workspace.
- README and CI use limxsdk-lowlevel master again: its `tron2.h` no
  longer includes unshipped `mros/hand_msgs` headers (fixed upstream on
  2026-10-27).
- `tron2_gazebo/launch/empty_world.launch`: load the URDF from
  `robot_description/tron2a/<robot_type>/`, the layout used by
  `tron2-robot-description`; the old `tron2/` path did not exist there.
- CI never ran: the workflow listened on `main` (default branch is
  `master`) and `ci.yml` was not valid YAML (unquoted `: ` in a step
  name). The doc-marker scan now ignores the grep command quoted in
  `CONTRIBUTING.md`. The build job, which targeted the retired
  `ubuntu-20.04` runner and only built this repository, now runs in a
  `ros:noetic` container, builds the full workspace with
  `tron2_rl_deploy_ros`, `limxsdk-lowlevel` and `tron2-robot-description`,
  expands both URDFs and runs a headless Gazebo smoke test.
- Distribution note (README EN/ZH) points at
  `github.com/limxdynamics/tron2_gazebo_ros` and no longer names the
  internal GitLab address.
- Stale license note in the README: both `package.xml` files declare
  Apache-2.0 (resolved 2026-07-14). The Scope section links the real
  sibling repositories instead of the non-existent `limx-tron2`
  organisation.
- Repository links in both `package.xml` files, `CONTRIBUTING.md` and the
  changelog footer point at `limxdynamics/tron2_gazebo_ros`.
- `.github/CODEOWNERS`: the `@limx-tron2/*` teams do not exist; all paths
  are owned by `@limxdynamics` until real teams are set up.
- `THIRD_PARTY_NOTICES.md` §5 lists the new rendered images and records
  how the existing media were re-encoded.

### Changed
- `README.md` / `README_zh-CN.md`: landing page with banner, model
  cards, how-it-works picture, highlights and quick start (new images
  `doc/banner.png`, `doc/model_{sf,wf}.png`, `doc/sim2real_{en,zh}.png`
  rendered from `tron2-robot-description`). Workspace setup now clones
  this repository instead of `tron1-gazebo-ros`, adds
  `tron2_rl_deploy_ros` and `tron2-robot-description`. New sections for the
  `tron2_hw_sim.launch` arguments and the gamepad mapping; two FAQ
  entries. "Cite & support" links point at this repository; License,
  Scope and Distribution text unchanged, License and Scope moved to the
  end.
- Real-world GIF captions corrected: `doc/wf.GIF` shows SF_TRON2A and
  `doc/sf.GIF` shows WF_TRON2A.
- `doc/banner.png` redrawn: one SF_TRON2A, half Gazebo hologram on a
  grid floor and half studio-lit real robot, joined by a "same
  controller" label, so the banner shows what the repository does.
  New `doc/banner_zh.png` (used by `README_zh-CN.md`) and
  `doc/social_preview.png` (1280x640, for Settings -> Social preview).
  Rendered from `tron2-robot-description`; no metadata chunks.
- Media: Gazebo GIFs cropped to the area the robot walks in and stripped
  of their converter comment; real-world GIFs center-cropped to 300x300
  at 8 fps; `doc/deploy.jpg` converted to sRGB without EXIF. `doc/`
  drops from about 100 MB to about 11 MB.
- `.github/ISSUE_TEMPLATE/config.yml`: "General questions" now points at
  the README FAQ (Discussions are disabled on this repository).
- `tron2_gazebo/package.xml`: `<version>` bumped from `0.0.0` to
  `0.1.0` for first public release; `<maintainer>` set to
  `LimX Dynamics <contact@limxdynamics.com>`.
- `limxsdk-sim/package.xml`: `<version>` bumped from `0.0.0` to
  `0.1.0`; `<maintainer>` normalised to
  `LimX Dynamics <contact@limxdynamics.com>`.
- ROS package names (`tron2_gazebo`, `limxsdk_sim`) intentionally
  **not** renamed — downstream `<depend>` declarations rely on them;
  any rename is tracked as a future breaking change and deferred.

### Resolved (2026-07-14)
- **Top-level `LICENSE` file** — added Apache-2.0. The full license
  text is now shipped at the repository root and matches the sibling
  `robot-description/LICENSE`.
- **`package.xml` `<license>` values** — both packages now declare
  `<license>Apache-2.0</license>`, consistent with the top-level
  LICENSE. The two `⚠ TO CONFIRM` comments have been removed.
  `NOTICE` and `THIRD_PARTY_NOTICES.md §1` reflect the resolution.

### Resolved (2026-07-16)
- **Private-IP handling** — resolved 2026-07-16 per owner decision.
  All Markdown / YAML command examples now use `<robot-ip>` as a
  placeholder token. The `limxsdk-sim/include/limxsdk/apibase.h`
  Doxygen comment retains the literal `10.192.1.2` as a
  documentation example only, declared in `SECURITY.md`. CI's
  private-IP scan continues to allowlist that one location.

### Pending owner sign-off (blocks first public tag)
- **Bundled SDK binaries** — provenance, SHA-256 checksums, and
  re-distribution license for
  `limxsdk-sim/lib/aarch64/liblimxsdk_sim.so`,
  `limxsdk-sim/lib/amd64/liblimxsdk_sim.so`,
  `limxsdk-sim/lib/arm32/liblimxsdk_sim.so`,
  `limxsdk-sim/lib/win/limxsdk_sim.dll`, and
  `limxsdk-sim/lib/win/limxsdk_sim.lib`
  (`THIRD_PARTY_NOTICES.md` §2).
- **`doc/` media review** — EXIF strip and content review of
  `doc/deploy.jpg`, `doc/sf.GIF`, `doc/sfgazebo-*.gif`, `doc/wf.GIF`,
  `doc/wfgazebo.gif` (`THIRD_PARTY_NOTICES.md` §5).
- **Robot control interface** — the joint effort / RobotCmd /
  RobotState bridge in `tron2_gazebo/src/Tron2HWSim.cpp` (see
  `SECURITY.md`) must be cleared by the robotics / SDK owner for
  public release.

## [0.1.0] — TBD

First public release. Contents:

- `tron2_gazebo` — Gazebo `gazebo_ros_control` plugin
  (`Tron2HWSim`) with hybrid joint interface, worlds, launch files,
  and controller configs.
- `limxsdk-sim` — simulation-side C++ SDK: headers under
  `include/limxsdk/` and pre-built binaries under
  `lib/{aarch64,amd64,arm32,win}/` (subject to owner sign-off, see
  Unreleased).
- ROS 1 (catkin) support in `package.xml` / `CMakeLists.txt`, with
  ROS 2 (`ament_cmake`) blocks kept for future work.

[Unreleased]: https://github.com/limxdynamics/tron2_gazebo_ros/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/limxdynamics/tron2_gazebo_ros/releases/tag/v0.1.0
