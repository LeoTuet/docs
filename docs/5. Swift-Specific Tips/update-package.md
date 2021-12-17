# Updating Swift Packages


Every time you add a Swift package, the Swift Package Manager will download the newest version of that package that conforms to your version requirements, and add a reference to the exact version number to it's [Package.resolved](https://github.com/apple/swift-package-manager/blob/main/Documentation/Usage.md#resolving-versions-packageresolved-file) file. Some people check this file into version control – in that case this ensures that the whole team is working with the exact same version of the package.

While most Swift packages adhere to a [Semantic Versioning Scheme](https://semver.org) that promises that no breaking changes occur in a point release, sometimes accidents still happen: A bug will slip into the newest version, or a change will break compatibility with your code. This is why Swift Package Manager, like most package managers, pins down the exact version being used in the `Package.resolved` file, and expects you to update your packages manually. 

When updating manually, you can run the update at a time of your choosing. This ensures that you are aware that something in your code has changed, and you can test your application and check if everything is still working as expected.

With that in mind, here's how to manually update a Swift package using Xcode:

## How to update a Swift Package in XCode

<img class="img img-fluid shadow rounded" alt="A screenshot of Xcode. The user has right-clicked on the TelemetryClient Package and selected 'Update Package'." src="/images/updatepackage.png">

Updating packages in Xcode can be done using one of two ways. If you want to just update a single package individually – which we recommend by the way – do this:

1. Navigate to the package you want to update in the Xcode navigator. It should be at the very bottom of the list, in a section called **Package Dependencies**.
2. Right-click the package you want to update.
3. Choose **Update Package**.

And that's it. 

You can also update every single Swift Package in your project at once. To do this, open the **Files** menu, navigate to **Packages** and then click **Update to Latest Package Versions**. Afterwards, make sure to test your project to see if everything still works, especially in projects with a lot of package dependencies.

## Troubleshooting

Sometimes Xcode's package cache gets confused. This will usually result in weird build errors that can't really be explained. Here are two things that might help:

1. **Resetting the Xcode Package Cache**: To reset the package cache, open the **File** menu, navigate to **Packages**, and click **Reset Package Caches**. This will delete all local package data and redownload each package from it's source online.
2. **Cleaning the Build Folder**: To clean your build folder, open the **Product** menu in Xcode, and click **Clean Build Folder**. This will clean the partial build caches, so that your next build will start from scratch. This can also sometimes help resolve an issue.