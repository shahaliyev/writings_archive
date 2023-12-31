---
title: A Guide to Open-source Software Licenses
categories: [computer science]
tags: [open-source, medium]
---

I need to use a 10-line code snippet from another project in my program, but as the author didn’t create any licensing file about software’s free-usage and distribution, I cannot. Sad.

According to the official [open-source definition](https://opensource.org/osd), ten features define an open-source software product, the first two of which are free redistribution and publicly available source code. Public repositories in Github are only complying with the second characteristic, as no license means default copyright laws: no one may reproduce, distribute, or create derivative work. Forking is only a Github-specific option mentioned in the terms of service and it doesn’t forego the default copyright of the owner.

It’s up to you to have your software open-source or not, but if you wish to have so, you need to provide an open-source license document to redefine your copyrights. You can even do it aggressively by immediately switching to copyleft, which is a real term and has its reversed copyright symbol (ɔ).

Having a license file in your repository makes you look credible when you actually are not. Another advantage of this file is that, without it, project contributors become exclusive copyright holders of their own work, which you might want to object against. From here, if you want other people to modify your project and contribute back to it, open-source licensing becomes essential.

It is a best practice to include the license terms in the LICENSE file with the project, in addition to the README file. Keep it simple, if you want to specify some complexities regarding the license agreement, you can do it so in the README file. Simplicity also helps Github’s Licenses API to [search for the license](https://github.blog/2017-11-03-search-repositories-by-license/) and filter software products.

For sure, one needs to discuss copyright details with a lawyer, but if you live a carefree life, there are a number of nice open-source licenses. The most popular one in Github is the [MIT](https://choosealicense.com/licenses/mit/) license for its simplicity and attitude, while [Apache 2.0](https://choosealicense.com/licenses/apache-2.0/) together with [GPLv3](https://choosealicense.com/licenses/gpl-3.0/) conclude the top three of the frequently used open-source licenses. There are also many other licenses to choose from and that will force you to constantly refer to the domain choosealicense(dot)com. You can also come up with your own license terms (ahem) if you want.

Every project you create will usually have dependencies on other libraries, when those libraries have their own copyright notice. If all of the dependencies have [permissive licenses](https://en.wikipedia.org/wiki/Permissive_software_license) (MIT, Apache 2.0, BSD, etc), then after redistribution, you can use any license you wish. Things are different with [copylefted](https://en.wikipedia.org/wiki/Copyleft) dependencies, when you will have to use the exact same license (GPLv3, AGPLv3, etc).

To simply differentiate between permissive and copylefted licenses, one should remember that copyleft doesn’t allow redistributed software to be non-open-source. Therefore, if you are a hardcore open-source proponent, you are obliged to go with copyleft GPL licenses.

I will now briefly introduce the most popular open-source licenses and their usage, but I knock down any responsibility regarding the correctness of this essay and the consequences of your future actions, obviously.

The [MIT license](https://choosealicense.com/licenses/mit/) allows doing all the dirty things to the source code as long as a copy of the license with copyright notice is provided. You can release the project under a different license later, but modifying old product licenses will require legal action. While using a code snippet from MIT-licensed software, it is enough to note the usage of it in the documentation and refer to the copy of the license (e.g. “The software uses this code snippet from this project, refer to the copy of the project’s license file.”)

We have discussed something about the MIT license and noted that GPL licenses follow strict copyleft notice, but let’s not forget about the existence of the Apache 2.0 license. It’s needed with the corporate world in mind, when businesses want their open-source project licenses to say something about patents and require a more strict distribution of the project.

Finally, if you have a commercial goal in your mind with your open-source software (attention please), licenses like the Eclipse Public License, Mozilla Public License 2.0, or Microsoft Reciprocal License should do the job. You need to pay attention to the patent statements and jurisdictions in each license, and you may need a lawyer to choose the correct license for your product.

In conclusion, I will re-introduce the legal notice for this essay: you cannot redistribute, modify, or delete this essay without the permission of the author. I reject every responsibility and deny all the warranty regarding the usefulness and consequences of this essay.

*   [https://opensource.org/osd](https://opensource.org/osd)
*   [https://opensource.com/law/13/1/which-open-source-software-license-should-i-use](https://opensource.com/law/13/1/which-open-source-software-license-should-i-use)
*   [choosealicense.com](https://choosealicense.com/)
*   [https://github.blog/2015-03-09-open-source-license-usage-on-github-com/](https://github.blog/2015-03-09-open-source-license-usage-on-github-com/)
*   [https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/licensing-a-repository](https://docs.github.com/en/free-pro-team@latest/github/creating-cloning-and-archiving-repositories/licensing-a-repository)
*   [https://www.gnu.org/copyleft/](https://www.gnu.org/copyleft/)