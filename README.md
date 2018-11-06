# Android-Unique-Application-ID
It is very common, and perfectly reasonable, for a developer to want to track individual installations of their apps. It sounds plausible just to call TelephonyManager.getDeviceId() and use that value to identify the installation. There are problems with this: First, it doesn’t work reliably (see below). Second, when it does work, that value survives device wipes (“Factory resets”) and thus you could end up making a nasty mistake when one of your customers wipes their device and passes it on to another person.

To track installations, you could for example use a UUID as an identifier, and simply create a new one the first time an app runs after installation. Here is a sketch of a class named “Installation” with one static method Installation.id(Context context). You could imagine writing more installation-specific data into the INSTALLATION file.



public class Installation {
    private static String sID = null;
    private static final String INSTALLATION = "INSTALLATION";

    public synchronized static String id(Context context) {
        if (sID == null) {  
            File installation = new File(context.getFilesDir(), INSTALLATION);
            try {
                if (!installation.exists())
                    writeInstallationFile(installation);
                sID = readInstallationFile(installation);
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }
        return sID;
    }

    private static String readInstallationFile(File installation) throws IOException {
        RandomAccessFile f = new RandomAccessFile(installation, "r");
        byte[] bytes = new byte[(int) f.length()];
        f.readFully(bytes);
        f.close();
        return new String(bytes);
    }

    private static void writeInstallationFile(File installation) throws IOException {
        FileOutputStream out = new FileOutputStream(installation);
        String id = UUID.randomUUID().toString();
        out.write(id.getBytes());
        out.close();
    }
}
