import java.io.Closeable;
import java.io.IOException;
import java.io.Reader;

public class StdBufferedReader implements Closeable {

    Reader reader;
    int n;
    int startNewLine = 0;
    boolean existNextLineInCbuf = false;

    char[] cbuf;

    public StdBufferedReader(Reader reader, int bufferSize) {
        if (reader == null) {
            throw new NullPointerException();
        }
        if (bufferSize <= 0) {
            throw new IllegalArgumentException();
        }

        this.reader = reader;
        this.cbuf = new char[bufferSize];
    }

    public StdBufferedReader(Reader reader) {
        this(reader, 64);
    }


    public boolean hasNext() throws IOException{
        return reader.ready() || existNextLineInCbuf;
    }

    // Returns a line (everything till the next line)
    public char[] readLine() throws IOException {

        char[] beforeSeparatorOrEndOfCbuf;
        char[] tempLine = new char[]{};
        char[] result = new char[]{};

        while (n != -1) {

            if (!existNextLineInCbuf) {
                if (this.hasNext()) {
                    n = reader.read(cbuf, 0, cbuf.length);
                    startNewLine = 0;
                } else {
                    result = tempLine;
                    return result;
                }
            }

            for (int i = startNewLine; i < n; i++) {
                if (cbuf[i] == '\n') {

                    existNextLineInCbuf = true;

                    beforeSeparatorOrEndOfCbuf = new char[i - startNewLine];
                    System.arraycopy(cbuf, startNewLine, beforeSeparatorOrEndOfCbuf, 0, beforeSeparatorOrEndOfCbuf.length);

                    result = (String.valueOf(tempLine) + String.valueOf(beforeSeparatorOrEndOfCbuf)).toCharArray();

                    startNewLine = i + 1;

                    return result;

                }
            }

            existNextLineInCbuf = false;

            beforeSeparatorOrEndOfCbuf = new char[n - startNewLine];
            System.arraycopy(cbuf, startNewLine, beforeSeparatorOrEndOfCbuf, 0, beforeSeparatorOrEndOfCbuf.length);
            tempLine = (String.valueOf(tempLine) + String.valueOf(beforeSeparatorOrEndOfCbuf)).toCharArray();
            result = tempLine;
        }
        return result;
    }


    // Closing
    public void close() throws IOException {
        reader.close();
    }


}
