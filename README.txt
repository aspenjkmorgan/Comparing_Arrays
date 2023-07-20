import edu.princeton.cs.algs4.*;
import edu.princeton.cs.algs4.In;
import edu.princeton.cs.algs4.Heap;

public class CompareTwoArrays
{

    private static boolean compareWithHeap (In inA, In inB, int size) {
        
        // want to use heapsort which has guarenteed 2NlogN and is in place
        Integer[] a = new Integer[size];
        Integer[] b = new Integer[size];
        boolean match = true;

        int i = 0;
        while (!inA.isEmpty()) {
            a[i] = inA.readInt();
            i++;
        }

        int j = 0;
        while (!inB.isEmpty()) {
            b[j] = inB.readInt();
            j++;
        }
        
        // heap sort
        Heap.sort(a);
        Heap.sort(b);

        // compare
        for (int k = 0; k < size; k++) {
            if (!a[k].equals(b[k])) {
                match = false;
                break;
            }
        }

        return match;
    }
    
    // numbers might not be distinct
    private static boolean compareWithHash (In inA, In inB, int size) {
        // linear extra space
        LinearProbingHashST<Integer, Integer> hashie = new LinearProbingHashST<>();
        boolean match = true;

        while (!inA.isEmpty()) {
            int key = inA.readInt();
            
            if (hashie.contains(key)) {
                hashie.put(key, hashie.get(key) + 1);
            } else {
                hashie.put(key, 1);
            }
        }

        while (!inB.isEmpty()){
            int item = inB.readInt();
            
            if (hashie.contains(item)) {
                int val = hashie.get(item);
                
                if (val == 1) {
                    hashie.delete(item);
                } else {
                    hashie.put(item, val - 1);
                }

            } else {
                match = false;
                break;
            }
        }
        
        return match;
    }
    
    public static void main(String[] args) {
        
        if (args.length < 3) {
            StdOut.println("Usage: java-algs4 CompareTwoArrays filenameA filenameB [heap/hash]");
            System.exit(1);
        }
        String filenameA = args[0];
        String filenameB = args[1];
        String method    = args[2];
        
        if ( !method.equals("heap") && !method.equals("hash") ) {
            StdOut.println("Usage: java-algs4 CompareTwoArrays filenameA filenameB [heap/hash]");
            System.exit(1);
        }
        
        In inA = new In(filenameA);
        int nA = inA.readInt();

        In inB = new In(filenameB);
        int nB = inB.readInt();

        if (nA != nB) {
            StdOut.println("Arrays are not the same size, so not equivalent");
            System.exit(0);
        }
        
        boolean match = false;
        
        StopwatchCPU sw = new StopwatchCPU();
        if (method.equals("heap")) {
            match = compareWithHeap(inA, inB, nA);
        } else {
            match = compareWithHash(inA, inB, nA);
        }
        
        
        double elapsed = sw.elapsedTime();
        
        
        StdOut.println("The two input arrays do " + (match?"":"not ") + "match" );
        StdOut.println("elapsed time: " + elapsed + " seconds");
        
    }

   
}

