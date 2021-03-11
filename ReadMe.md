# Tye with Dapr

**Installation**: https://www.nuget.org/packages/Microsoft.Tye

**Instructions**: https://github.com/dotnet/tye/blob/master/docs/recipes/dapr.md

**Steps**:
- Run `tye init` at the solution level to create `tye.yml` file
- Under `name` add:

    ```yaml
    extensions:
    - name: dapr
    ```

Run at the solution level: `tye run`

**Dashboard**: http://localhost:8000/
- Click on service bindings to view app or web api.
- Append `swagger` or `weatherforecast` to web api url.

**Debugging**: https://dev.to/thangchung/debugging-dapr-application-using-tye-tool-1djb
- Run `tye run --debug Backend/Backend.csproj`
- In Visual Studio attach to both the FrontEnd.exe and Backend.exe processes.
- Set breakpoints and debug.