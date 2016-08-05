---

layout: blog/post
tags: freya hopac concurrency
title: Freya + Hopac

---

Starting with 3.0 (including release candidate builds), we're very excited that Freya now supports two concurrency models in the F# ecosystem. Traditionally, Freya has been based around the F# `async` abstraction, but as of 3.0 you can choose between using the "default" implementation or an implementation integrating [Hopac][hopac]. If you write your systems using the fantastic concurrent programming options offered by Hopac, you can now join this up seamlessly with Freya.

As of 3.0, new packages have been introduced, including for the Freya meta-package (giving a complete Freya stack). Simply select **Freya.Hopac** instead of **Freya** as a dependency in your package management tool of choice, and start using Hopac.

For more on the packages available and other relevant information, see the [Hopac section][freya-hopac] in the Freya documentation.

[freya-hopac]: https://docs.freya.io/en/latest/topics/general/hopac.html
[hopac]: https://github.com/Hopac/Hopac
