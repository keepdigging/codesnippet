> based on version 3.0.1

#### core code

    public static void main(String[] args) throws Exception
    {
        IndexSearcher indexSearcher = null;
        try {
            indexSearcher = new IndexSearcher(FSDirectory.open(new File(INDEX_DIR)));
            TopDocs topDocs = indexSearcher.search(
                    new QueryParser(Version.LUCENE_30, "contents", new StandardAnalyzer(Version.LUCENE_30))
                            .parse("conditions"),
                    100);

            System.out.println("检索到几个: " + topDocs.totalHits);
            System.out.println("最大得分: " + topDocs.getMaxScore());

            for (ScoreDoc scoreDoc : topDocs.scoreDocs)
            {//无序
                System.out.print(scoreDoc.toString());

                Document document = indexSearcher.doc(scoreDoc.doc);
                System.out.print(" - " + document.getField("fullpath").stringValue());
                System.out.print(" - " + document.getField("filename").stringValue());

                System.out.println();
            }
        }finally
        {
            if(indexSearcher != null)
            {
                indexSearcher.close();
            }
        }
    }


---

