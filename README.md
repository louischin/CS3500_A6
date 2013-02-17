CS3500_A6
=========

Black Box Testing

This is what I had for my code, I had inadequate tests, 
and bad09 which was extra spaces for list of size > 1


public class TestFListDouble {

    // Runs the tests.
               
    public static void main(String args[]) {
        TestFListDouble test = new TestFListDouble();

        // Test with 0-argument FListDouble.emptyList().

        System.out.println("Testing 0-argument emptyList()");
        test.creation();
        test.accessors(0);
        test.usual();
        test.accessors(0);    // test twice to detect side effects
        test.usual();

        test.summarize();
    }

    // Prints a summary of the tests.

    private void summarize () {
        System.out.println();
        System.out.println (totalErrors + " errors found in " +
                            totalTests + " tests.");
    }

    public TestFListDouble () { }

    // Double objects to serve as elements.

    private final Double one   = new Double(1.0);
    private final Double two   = new Double(2.0);
    private final Double three = new Double(3.0);
    private final Double four  = new Double(4.0);
    private final Double five  = new Double(5.0);
    private final Double six   = new Double(6.0);

    // FListDouble objects to be created and then tested.

    private FListDouble f0;    // { }
    private FListDouble f1;    // { 1.0 }
    private FListDouble f2;    // { 2.0, 1.0 }
    private FListDouble f3;    // { 3.0, 2.0, 1.0 }
    private FListDouble f4;    // { 4.0, 3.0, 2.0, 1.0 }
    private FListDouble f5;    // { 1.0, 2.0, 1.0 }
    private FListDouble f6;    // { 3.0, 4.0, 2.0, 1.0 }
    private FListDouble f7;    // { 1.0, 2.0, 2.0, 1.0 }

    private FListDouble s1;    // { 1.0 }
    private FListDouble s2;    // { 2.0 }
    private FListDouble s3;    // { 3.0 }
    private FListDouble s4;    // { 4.0 }

    private FListDouble s12;    // { 1.0, 2.0 }
    private FListDouble s13;    // { 1.0, 3.0 }
    private FListDouble s14;    // { 1.0, 4.0 }
    private FListDouble s23;    // { 2.0, 3.0 }
    private FListDouble s34;    // { 3.0, 4.0 }

    private FListDouble s123;   // { 1.0, 2.0, 3.0 }
    private FListDouble s124;   // { 1.0, 2.0, 4.0 }
    private FListDouble s134;   // { 1.0, 3.0, 4.0 }
    private FListDouble s234;   // { 2.0, 3.0, 4.0 }

    // Creates some FListDouble objects.

    private void creation () {
        try {
            f0 = FListDouble.emptyList();
            f1 = FListDouble.add (f0, one);
            f2 = FListDouble.add (f1, two);
            f3 = FListDouble.add (f2, three);
            f4 = FListDouble.add (f3, four);
            f5 = FListDouble.add (f2, one);
            f6 = FListDouble.add (FListDouble.add (f2, four), three);

            f7 = FListDouble.add (f0, one);
            f7 = FListDouble.add (f7, two);
            f7 = FListDouble.add (f7, two);
            f7 = FListDouble.add (f7, one);

            s1 = FListDouble.add (f0, one);
            s2 = FListDouble.add (f0, two);
            s3 = FListDouble.add (f0, three);
            s4 = FListDouble.add (f0, four);

            s12 = FListDouble.add (f1, new Double(2));
            s13 = FListDouble.add (f1, new Double(3));
            s14 = FListDouble.add (f1, new Double(4));
            s23 = FListDouble.add (s2, new Double(3));
            s34 = FListDouble.add (s3, new Double(4));

            s123 = FListDouble.add (s12, three);
            s124 = FListDouble.add (s12, four);
            s134 = FListDouble.add (s13, four);
            s234 = FListDouble.add (s23, four);

            s134 = FListDouble.add (s134, four);
            s234 = FListDouble.add (s234, four);

        }
        catch (Exception e) {
            System.out.println("Exception thrown during creation tests:");
            System.out.println(e);
            assertTrue ("creation", false);
        }
    }

    // Tests the accessors.

    private void accessors (int nargs) {
        try {
            assertTrue ("empty", FListDouble.isEmpty (f0));
            assertFalse ("nonempty", FListDouble.isEmpty (f1));
            assertFalse ("nonempty", FListDouble.isEmpty (f3));

            assertTrue ("f0.size()", FListDouble.size (f0) == 0);
            assertTrue ("f1.size()", FListDouble.size (f1) == 1);
            assertTrue ("f2.size()", FListDouble.size (f2) == 2);
            assertTrue ("f3.size()", FListDouble.size (f3) == 3);
            assertTrue ("f4.size()", FListDouble.size (f4) == 4);
            assertTrue ("f5.size()", FListDouble.size (f5) == 3);
            assertTrue ("f7.size()", FListDouble.size (f7) == 4);

            assertTrue ("get11", FListDouble.get(f1, 0).equals
            (FListDouble.get(s1, 0)));
            assertTrue ("get32", FListDouble.get(f3, 1).equals
            (FListDouble.get(s2, 0)));
            assertTrue ("get42", FListDouble.get(f4, 2).equals
            (FListDouble.get(f2, 0)));
            assertTrue ("get123", FListDouble.get(s123, 1).equals
            (FListDouble.get(s124, 1)));
            assertTrue ("get14", FListDouble.get(s14, 0).equals
            (FListDouble.get(s34, 0)));
            assertTrue ("get33", FListDouble.get(f3, 0).equals
            (FListDouble.get(s3, 0)));
            assertFalse ("get35", FListDouble.get(s4, 0).equals
            (FListDouble.get(s123, 0)));
            assertFalse ("get23", FListDouble.get(f2, 0).equals
            (FListDouble.get(s3, 0)));
            assertFalse ("get1234", FListDouble.get(s1, 0).equals
            (FListDouble.get(s234, 0)));
            
            
            assertTrue ("set11", FListDouble.set(f1, 0, two).equals
            (FListDouble.set(s1, 0, two)));
            assertTrue ("set22", FListDouble.set(s12, 0, two).equals
            (FListDouble.set(s13, 0, two)));
            assertTrue ("set33", FListDouble.set(s13, 1, one).equals
            (FListDouble.set(s23, 1, one)));
            assertTrue ("set34", FListDouble.set(s123, 0, three).equals
            (FListDouble.set(s124, 0, three)));
            assertTrue ("set14", FListDouble.set(s14, 1, five).equals
            (FListDouble.set(s34, 1, five)));
            assertFalse ("set12", FListDouble.set(s12, 1, two).equals
            (FListDouble.set(s13, 0, two)));
            assertFalse ("set123", FListDouble.set(s12, 1, two).equals
            (FListDouble.set(s23, 1, two)));
            assertFalse ("set234", FListDouble.set(s234, 2, two).equals
            (FListDouble.set(s14, 0, three)));
            assertFalse ("set37", FListDouble.set(f3, 1, one).equals
            (FListDouble.set(f7, 3, two)));

            assertFalse ("contains01", FListDouble.contains (f0, one));
            assertFalse ("contains04", FListDouble.contains (f0, four));
            assertTrue  ("contains11", FListDouble.contains (f1, one));
            assertTrue  ("contains11new", 
              		FListDouble.contains (f1, new Double(one)));
            assertFalse ("contains14", FListDouble.contains (f1, four));
            assertTrue  ("contains21", FListDouble.contains (f2, one));
            assertFalse ("contains24", FListDouble.contains (f2, four));
            assertTrue  ("contains31", FListDouble.contains (f3, one));
            assertFalse ("contains34", FListDouble.contains (f3, four));
            assertTrue  ("contains41", FListDouble.contains (f4, one));
            assertTrue  ("contains44", FListDouble.contains (f4, four));
            assertTrue  ("contains51", FListDouble.contains (f5, one));
            assertFalse ("contains54", FListDouble.contains (f5, four));
           
        }
        catch (Exception e) {
            System.out.println("Exception thrown during accessors tests:");
            System.out.println(e);
            assertTrue ("accessors", false);
        }
    }

    // Tests the usual overridden methods.

    private void usual () {
        try {
            assertTrue ("toString0",
                        f0.toString().equals("[]"));
            assertTrue ("toString1",
                        f1.toString().equals(s1.toString()));
            assertTrue ("toString123", s123.toString().equals
            		(FListDouble.add(s12, three).toString()));

            assertTrue ("equals00", f0.equals(f0));
            assertTrue ("equals33", f3.equals(f3));
            assertTrue ("equals55", f5.equals(f5));
            assertTrue ("equals44", four.equals(four));

            assertFalse ("equals01", f0.equals(f1));
            assertFalse ("equals02", f0.equals(f2));
            assertFalse ("equals10", f1.equals(f0));
            assertFalse ("equals12", f1.equals(f2));
            assertFalse ("equals21", f2.equals(f1));
            assertFalse ("equals23", f2.equals(f3));
            assertFalse ("equals35", f3.equals(f5));

            assertFalse ("equals0string", f0.equals("just a string"));
            assertFalse ("equals4string", f4.equals("just a string"));

            assertFalse ("equals0null", f0.equals(null));
            assertFalse ("equals1null", f1.equals(null));

            assertTrue ("hashCode00", f0.hashCode() == f0.hashCode());
            assertTrue ("hashCode44", f4.hashCode() == f4.hashCode());
            assertTrue ("hashCode46", f1.hashCode() == s1.hashCode());
            assertTrue ("hashCode124", s124.hashCode() == 
            		FListDouble.add(s12, four).hashCode());

        }
        catch (Exception e) {
            System.out.println("Exception thrown during usual tests:");
            System.out.println(e);
            assertTrue ("usual", false);
        }
    }

////////////////////////////////////////////////////////////////

    private int totalTests = 0;       // tests run so far
    private int totalErrors = 0;      // errors so far

    // For anonymous tests.  Deprecated.

    private void assertTrue (boolean result) {
      assertTrue ("anonymous", result);
    }

    // Prints failure report if the result is not true.

    private void assertTrue (String name, boolean result) {
        if (! result) {
            System.out.println ();
            System.out.println ("***** Test failed ***** "
                                + name + ": " +totalTests);
            totalErrors = totalErrors + 1;
        }
        totalTests = totalTests + 1;
    }

    // For anonymous tests.  Deprecated.

    private void assertFalse (boolean result) {
        assertTrue (! result);
    }

    // Prints failure report if the result is not false.

    private void assertFalse (String name, boolean result) {
        assertTrue (name, ! result);
    }

}
