using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

namespace StringHelper
{
    public class HtmlFormatterHelper
    {
        private string classDenom = "class=\"cl";//cl1,cl2,cl3,cl(n)
        private string classStyleSectionHeader = "<style type=\"text/css\">";
        public string OptimizeHtml(string source)
        {
            return OptimizeHtml(source, new List<string>());
        }


        public string OptimizeHtml(string source, List<string> AttibutesRemove, bool removeSpaces = false, bool removeComments = false)
        {
            string retail = source;
            //Isolate Styles
            var styles = GetStyles(source);

            //Exchange styles for their respective classes
            foreach (var style in styles)
            {
                string match = "style=\"" + style.Value + "\"";

                source = source.Replace(match, classDenom + style.Key + "\"");

            }

            //check if the source already have the style section
            var hasStyleSection = retail.Contains(classStyleSectionHeader);

            string classComposition = "<style type=\"text/css\">";
            foreach (var style in styles)
            {
                string stl = ".cl" + style.Key + "{" + style.Value + "}";
                classComposition += stl;
            }
            classComposition += "</style>";

            if (hasStyleSection)
            {
                source = source.Replace(classStyleSectionHeader, classComposition);
            }
            else
            {
                source = classComposition + source;
            }

            //remove attributes if given
            foreach (var attr in AttibutesRemove)
            {
                if (source.Contains(attr))
                {
                    source = TokenRemover(source, attr);
                }
            }

            //remove markup spaces
            if (removeSpaces)
            {
                source = Regex.Replace(source, @"\s*(?<capture><(?<markUp>\w+)>.*<\/\k<markUp>>)\s*", "${capture}", RegexOptions.Singleline);
            }

            //remove comments
            if (removeComments)
            {
                source = Regex.Replace(source, @"/\*.+?\*/", string.Empty, RegexOptions.Singleline);
                source = Regex.Replace(source, @"<!--(\n|.)*?-->", string.Empty);
            }
            return source;
        }

        private string TokenRemover(string source, string match)
        {          
            var results = source.Split(new string[] { match }, StringSplitOptions.None);
            source = source.Replace(match, "");
            foreach (var res in results.Skip(1))
            {
                var junkVal = res.Split('"')[0];
                source = source.Replace(junkVal, "");
            }

            return source;
        }

        private Dictionary<int, string> GetStyles(string source)
        {
            int i = 0;
            Dictionary<int, string> styles = new Dictionary<int, string>();
            var split = source.Split(new string[] { "style=\"" }, StringSplitOptions.None);
            foreach (var item in split)
            {
                //check if the element is the style section
                //todo remove
                if (item.Contains("style"))
                {
                    continue;
                }
                var stl = item.Split('"')[0];
                if (!styles.ContainsValue(stl))
                {
                    styles.Add(i, stl);
                    i++;
                }
            }

            return styles;
        }
    }
}
