> based on version 3.0.1

#### core code

    private static long buildIndex(String indexDir, String dataDir) throws Exception
    {
        IndexWriter writer = null;
        long numDocs;
        try {
            writer = new IndexWriter(
                    FSDirectory.open(new File(indexDir)),
                    new StandardAnalyzer(Version.LUCENE_30),
                    true,
                    IndexWriter.MaxFieldLength.UNLIMITED);

            for (File file: new File(dataDir).listFiles())
            {
                Document doc = new Document();
                doc.add(new Field("contents", new FileReader(file)));
                doc.add(new Field("filename", file.getName(), Field.Store.YES, Field.Index.NOT_ANALYZED));
                doc.add(new Field("fullpath", file.getCanonicalPath(), Field.Store.YES, Field.Index.NOT_ANALYZED));

                writer.addDocument(doc);
            }
            numDocs = writer.numDocs();
        }finally
        {
            if(writer != null)
            {
                writer.close();
            }
        }
        return numDocs;
    }


---


