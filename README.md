# relayinternalwcf
Azure Service Bus Relay Demonstration

Configure On-prem WCF Service

1.  In the WCF Service project, install NuGet package:  ``` Install-Package WindowsAzure.ServiceBus ```
2.  A bunch of extensions will be added into your projects' web.config file
3.  Update web.config.  Main parts are: service endpoint, endpoint behavior and binding configuration (netTcpRelayBinding in this example)

``` 
  <system.serviceModel>
    <services>
      <service name="SurfSpots.SanDiegoSpots">
        <endpoint address="sb://relaytester.servicebus.windows.net/surfspot"
          binding="netTcpRelayBinding"
          bindingConfiguration="sbRelayBinding"
          contract="SurfSpots.Contracts.ISanDiegoSpots"
          behaviorConfiguration="sbTokenProvider" />
      </service>
    </services>
    <bindings>
      <netTcpRelayBinding>
        <binding name="sbRelayBinding" connectionMode="Relayed" maxReceivedMessageSize="500000" transferMode="Buffered">
          <security mode="None">
            <transport></transport>
          </security>
        </binding>
      </netTcpRelayBinding>
    </bindings>
    <behaviors>
      <serviceBehaviors>
        <behavior>
          <!-- To avoid disclosing metadata information, set the values below to false before deployment -->
          <serviceMetadata httpGetEnabled="true" httpsGetEnabled="true"/>
          <!-- To receive exception details in faults for debugging purposes, set the value below to true.  Set to false before deployment to avoid disclosing exception information -->
          <serviceDebug includeExceptionDetailInFaults="false"/>
        </behavior>
      </serviceBehaviors>
      <endpointBehaviors>
        <behavior name="sbTokenProvider">
          <transportClientEndpointBehavior>
            <tokenProvider>
              <sharedAccessSignature keyName="RootManageSharedAccessKey" key="tveyH2hVHrm9+mtgqUGbZ+M7xcyXbH/twhB4P2Qvx1o=" />
            </tokenProvider>
          </transportClientEndpointBehavior>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    <protocolMapping>
        <add binding="basicHttpsBinding" scheme="https"/>
    </protocolMapping>    
    <serviceHostingEnvironment aspNetCompatibilityEnabled="true" multipleSiteBindingsEnabled="true"/>
    <extensions>
      <!-- In this extension section we are introducing all known service bus extensions. User can remove the ones they don't need. -->
      <behaviorExtensions>
        <!-- Too many to list here -->
    </extensions>
  </system.serviceModel>
```
