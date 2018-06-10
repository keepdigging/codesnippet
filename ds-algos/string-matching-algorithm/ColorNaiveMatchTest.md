    <dependency>
        <groupId>org.fusesource.jansi</groupId>
        <artifactId>jansi</artifactId>
        <version>1.17</version>
    </dependency>
    
    import java.util.ArrayList;
    import java.util.HashMap;
    import java.util.List;
    import java.util.Map;

    import static org.fusesource.jansi.Ansi.Color.GREEN;
    import static org.fusesource.jansi.Ansi.ansi;

    /**
    * 朴素的模式匹配,匹配多次,被匹配的字符串黄色加亮显示
    */
    public class ColorNaiveMatchTest {

        public static void main(String[] args) {

            String text = "welcome to the fucking world, Oh, I can't say the fucking word was fucked.";
            String pattern = "fuck";

            //naiveMatch(text.toCharArray(), pattern.toCharArray(), true);
            naiveMatch(text.toCharArray(), pattern.toCharArray(), false);

        }

        /**
        * @param text
        * @param pattern
        * @param existOnce
        */
        private static void naiveMatch(char[] text, char[] pattern, boolean existOnce) {

            List<Map<String, Integer>> matchIndexList = new ArrayList<Map<String, Integer>>();

            if (pattern.length > text.length) {
                doColorPrint(text, matchIndexList);
                return;
            }
            int i = 0, j = 0;
            while (i < text.length) {
                if (j == pattern.length) {
                    Map<String, Integer> map = new HashMap<String, Integer>();
                    map.put("begin", i - j);
                    map.put("end", i - 1);
                    matchIndexList.add(map);
                    if (existOnce) {
                        break;
                    }
                    j = 0;
                }
                if (text[i] == pattern[j]) {
                    i++;
                    j++;
                } else {
                    j = 0;
                    i = i - j + 1;
                }
            }

            doColorPrint(text, matchIndexList);
        }

        /**
        * @param text
        * @param indexList
        */
        private static void doColorPrint(char[] text, List<Map<String, Integer>> indexList) {

            if (indexList == null || indexList.size() <= 0) {
                System.out.println(text);
                return;
            }
            StringBuilder defaultColor = new StringBuilder();
            StringBuilder yellowColor = new StringBuilder();
            for (int i = 0; i < text.length; i++) {

                if (betweenMap(i, indexList)) {
                    if (defaultColor.length() > 0) {
                        System.out.print(defaultColor);
                        defaultColor.delete(0, defaultColor.length());
                    }
                    yellowColor.append(text[i]);
                } else {
                    if (yellowColor.length() > 0) {
                        System.out.print(
                                ansi().eraseScreen()
                                        .fg(GREEN).a(yellowColor)
                                        .reset()
                        );
                        yellowColor.delete(0, yellowColor.length());
                    }
                    defaultColor.append(text[i]);
                }
            }
            // flush remaining chars in buffer.
            if (yellowColor.length() > 0) {
                System.out.print(
                        ansi().eraseScreen()
                                .fg(GREEN).a(yellowColor)
                                .reset()
                );
                yellowColor.delete(0, yellowColor.length());
            }
            if (defaultColor.length() > 0) {
                System.out.print(defaultColor);
                defaultColor.delete(0, defaultColor.length());
            }
            System.out.println();
        }

        /**
        * @param valueIndex
        * @param indexList
        * @return
        */
        private static boolean betweenMap(int valueIndex, List<Map<String, Integer>> indexList) {
            for (Map<String, Integer> indexMap : indexList) {
                int beginIndex = indexMap.get("begin");
                int endIndex = indexMap.get("end");
                if (valueIndex >= beginIndex && valueIndex <= endIndex) {
                    return true;
                }
            }
            return false;
        }
    }

    welcome to the fucking world, Oh, I can't say the fucking word was fucked.
    