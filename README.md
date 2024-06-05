# Automic-UC4-Sample

Here's an example of how you can encapsulate this UC4 script within an XML format suitable for importing into Automic Automation (UC4).

### UC4 Job XML

This example creates a simple Unix job that sets an environment variable, captures its output, and uses it in the post-process section.

```xml
<uc-export>
    <header version="12.3.0+build.1599732629202" client="100" object_version="0" date_format="MM/DD/YYYY" time_format="HH:MM:SS" default_language="E">
        <created by="UC_USER" on="2024-06-05" />
        <modified by="UC_USER" on="2024-06-05" />
    </header>
    <object type="JOB" name="UNIX_ENV_VAR_JOB" folder="/" title="Job to Capture and Use Environment Variable">
        <attributes>
            <jobAttributes jobType="UNIX" subtype="" />
            <attribute name="Host" value="YOUR_HOST" />
            <attribute name="Login" value="YOUR_LOGIN" />
        </attributes>
        <script>
            <![CDATA[
! Set the environment variable using Unix command
:SET &CMD# = "sh -c 'export MY_VAR=my_value; echo $MY_VAR'"

! Execute the command and capture the output
:SET &PROCESS# = PREP_PROCESS(&CMD#)

! Initialize a variable to store the output
:SET &OUTPUT_VARIABLE# = ''

! Process the output line by line
:PROCESS &PROCESS#
:  SET &LINE# = GET_PROCESS_LINE(&PROCESS#, 1)
:  PRINT "Output: &LINE#"
:  SET &OUTPUT_VARIABLE# = STR_CAT(&OUTPUT_VARIABLE#, &LINE#, ' ')
:ENDPROCESS

! Set the UC4 variable to the captured output
:PUT_ATT OUTPUT_VARIABLE = &OUTPUT_VARIABLE#
            ]]>
        </script>
        <post_process>
            <![CDATA[
! Post-process section
:PRINT "Using the environment variable in post-process: &OUTPUT_VARIABLE#"
            ]]>
        </post_process>
    </object>
</uc-export>
```

### Explanation

1. **Header Section**:
   - Contains metadata about the export, such as version, client, and creation/modification details.

2. **Object Section**:
   - Specifies the type of object (`JOB`), its name (`UNIX_ENV_VAR_JOB`), and its folder location (`/`).

3. **Attributes Section**:
   - Specifies attributes for the Unix job, such as the host and login credentials. Replace `YOUR_HOST` and `YOUR_LOGIN` with appropriate values.

4. **Script Section**:
   - Contains the main script that sets the environment variable and captures its output.

5. **Post-Process Section**:
   - Contains the post-process script that prints the captured environment variable.

### Usage

1. **Save the XML**: Save the above XML content into a file, for example, `unix_env_var_job.xml`.
2. **Import into UC4**: Use the UC4 import functionality to import this XML file into your Automic Automation instance.

### Next Steps

**a.** Test the job to ensure it correctly captures and uses the environment variable.
**b.** Adjust the host and login attributes to match your environment setup.
