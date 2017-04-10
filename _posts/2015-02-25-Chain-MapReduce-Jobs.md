---
title: Chain MapReduce jobs in Java
layout: post
---

tags: MapReduce

Below is the sample code.

```java
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.util.Tool;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.fs.FileUtil;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.mapreduce.lib.input.TextInputFormat;
import org.apache.hadoop.mapreduce.lib.output.TextOutputFormat;
import org.apache.hadoop.util.ToolRunner;

public class KMeans extends Configured implements Tool
{
    @Override
    public int run(String[] args) throws Exception
    {
        System.out.println(Arrays.toString(args));

        int nIter = Integer.parseInt(args[2]);

        Path pInput = new Path(args[0]);
        Path dirCC = (new Path(args[3])).getParent();
        String fClusterCenters = args[3];

        for (int i = 1; i <= nIter; ++i) {
            Job job = Job.getInstance(getConf(), "KMeans");
            job.setJarByClass(KMeans.class);

            job.getConfiguration().setInt("kmeans.iteration.id", i);
            job.addCacheFile(new URI(fClusterCenters + "#cluster_centers"));

            job.setMapperClass(KMeansMapper.class);
            job.setCombinerClass(KMeansReducer.class);
            job.setReducerClass(KMeansReducer.class);

            job.setOutputKeyClass(IntWritable.class);
            job.setOutputValueClass(DoubleArrayWritable.class);

            job.setInputFormatClass(TextInputFormat.class);
            job.setOutputFormatClass(TextOutputFormat.class);

            Path pOutput = new Path(args[1] + "_" + i);
            FileInputFormat.addInputPath(job, pInput);
            FileOutputFormat.setOutputPath(job, pOutput);

            if (!job.waitForCompletion(true))
                return 1;

            // Merge output files into one
            FileSystem fs = FileSystem.get(job.getConfiguration());
            Path pMergedOutput = new Path(dirCC, "cluster_centers_" + i);
            if (fs.exists(pMergedOutput))
                fs.delete(pMergedOutput, false);

            FileUtil.copyMerge(fs, pOutput,
                               fs, pMergedOutput,
                               false,
                               job.getConfiguration(),
                               "");

            fClusterCenters = pMergedOutput.toString();
        }

        return 0;
    }

    public static void main(String[] args) throws Exception {
        System.out.println(Arrays.toString(args));
        int res = ToolRunner.run(new Configuration(),
                                 new KMeans(),
                                 args);
        System.exit(res);
    }

    // ...
}
```

Then in the mapper/reducer classes, we could add

```java
    public static class KMeansMapper
        extends Mapper<LongWritable, Text, IntWritable, DoubleArrayWritable>
    {
        private IntWritable mapkey = new IntWritable();
        private DoubleArrayWritable mapvalue = new DoubleArrayWritable();

        private List<double[]> clusterCenters = new ArrayList<double[]>();

        @Override
        protected void setup(Context context)
            throws IOException, InterruptedException, IllegalArgumentException
        {
            Configuration conf = context.getConfiguration();
            int iterId = conf.getInt("kmeans.iteration.id", 0);
            int offs = (iterId > 1)? 3 : 0;

            BufferedReader r = new BufferedReader(new FileReader("./cluster_centers"));
            String line;
            while ((line = r.readLine()) != null) {
                String[] coord = line.split("\\s+");
                double[] a = new double[coord.length - offs];
                for (int i = 0; i < coord.length - offs; ++i)
                    a[i] = Double.parseDouble(coord[i + offs]);
                clusterCenters.add(a);
            }

            if (clusterCenters.isEmpty())
                throw new IllegalArgumentException("Zero input cluster centers");
        }

	// ...
	}
```
