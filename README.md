# Magellanic.ServoController
Implementation of servo controller for the Raspberry Pi 3 with Windows IoT

Library to allow a Raspberry Pi 3 with Windows IoT to control a Servo directly
------------------------------------------------------------------------------

This has been tested with the Raspberry Pi 3 and Windows IoT, Insider Preview v14322.

WARNING: Controlling servos directly from the Raspberry Pi isn't a recommended action - servos are power hungry, and trying to drive them directly from the Raspberry Pi could lead to brown-outs, or other damage. You use this library at your own risk!

Preliminary requirements
------------------------

1. Enable the Microsoft Lightning Provider's "Direct Memory Mapped Driver" through the Pi's web interface - described under the "Runtime Requirements" heading at the URL: https://developer.microsoft.com/en-us/windows/iot/win10/LightningProviders.htm

2. In your Windows UWP project, change your package.appxmanifest to enable the necessary capabilities

    a. Change the Package root node to include the xmlns.iot namespace, and add "iot" to the Ignorable Namespaces, i.e.

    ```xml
    <Package
        xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
        xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
        xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
        xmlns:iot="http://schemas.microsoft.com/appx/manifest/iot/windows10"
             IgnorableNamespaces="uap mp iot">
    ```

    b. Add the iot:Capability and DeviceCapability to the capabilities node, i.e.

    ```xml
    <Capabilities>
        <iot:Capability Name="lowLevelDevices" />
        <DeviceCapability Name="109b86ad-f53d-4b76-aa5f-821e2ddf2141" />
    </Capabilities>
    ```

3. In your Windows UWP project:
    a. Open the Reference Manager (to open the reference manager, right click on your project's references and select "Add reference...");
    b. Expand "Universal Windows";
    c. Select "Extensions";
    d. Enable the "Windows IoT Extensions for the UWP";
    e. Click "OK".

Sample Code
-----------

Assuming that you've connected your servo's control line to Pin GPIO 5 (pin 29) - then in your project's MainPage class, you can call a method like:

        private async void MoveServoToCentre()
        {
            using (var servo = new ServoController(5))
            {
                await servo.Connect();

                servo.SetPosition(90).AllowTimeToMove(1000).Go();
            }
        }
