---
title: "Eclipse Extension ID 정리"
tags: [eclipse, osgi, rap, rcp]
date: 2010-12-27T16:17:45+09:00
---

**[Views]**  

1. Ant  
view id = org.eclipse.ant.ui.views.AntView  
context menu id = org.eclipse.ant.ui.views.AntView  
2. Bookmark  
view id = org.eclipse.ui.views.BookmarkView  
context menu id = org.eclipse.ui.views.BookmarkView  
3. Breakpoints  
view id = org.eclipse.debug.ui.BreakpointView  
context menu id = org.eclipse.debug.ui.BreakpointView  
4. Console  
view id = org.eclipse.ui.console.ConsoleView  
context menu id = org.eclipse.ui.console.ConsoleView  
5. Debug  
view id = org.eclipse.debug.ui.DebugView  
context menu id = org.eclipse.debug.ui.DebugView  
6. Display  
view id = org.eclipse.jdt.debug.ui.DisplayView  
context menu id = org.eclipse.jdt.debug.ui.DisplayView  
7. Expressions  
view id = org.eclipse.debug.ui.ExpressionView  
context menu id = org.eclipse.debug.ui.ExpressionView, org.eclipse.debug.ui.VariableView.detail  
8. Members  
view id = org.eclipse.jdt.ui.MembersView  
context menu id = org.eclipse.jdt.ui.MembersView  
9. Memory  
view id = org.eclipse.debug.ui.MemoryView  
context menu id = org.eclipse.debug.ui.MemoryView  
10. Navigator  
view id = org.eclipse.ui.views.ResourceNavigator  
context menu id = org.eclipse.ui.views.ResourceNavigator  
11. Package Explorer  
view id = org.eclipse.jdt.ui.PackageExplorer  
context menu id = org.eclipse.jdt.ui.PackageExplorer  
12. Packages  
view id = org.eclipse.jdt.ui.PackagesView  
context menu id = org.eclipse.jdt.ui.PackagesView  
13. Problems  
view id = org.eclipse.ui.views.ProblemView  
context menu id = org.eclipse.ui.views.ProblemView  
14. Projects  
view id = org.eclipse.jdt.ui.ProjectsView  
context menu id = org.eclipse.jdt.ui.ProjectsView  
15. Registers  
view id = org.eclipse.debug.ui.RegisterView  
context menu id = org.eclipse.debug.ui.RegisterView, org.eclipse.debug.ui.VariableView.detail  
16. Tasks  
view id = org.eclipse.ui.views.TaskList  
context menu id = org.eclipse.ui.views.TaskList  
17. Types  
view id = org.eclipse.jdt.ui.TypesView  
context menu id = org.eclipse.jdt.ui.TypesView  
18. Variables  
view id = org.eclipse.debug.ui.VariableView  
context menu id = org.eclipse.debug.ui.VariableView, org.eclipse.debug.ui.VariableView.detail  
  
**[Editors]**  
  
1. Ant Editor  
editor id = org.eclipse.ant.ui.internal.editor.AntEditor  
context menu id = #TextEditorContext, #TextRulerContext  
2. Class File Editor (\*.class)  
editor id = org.eclipse.jdt.ui.ClassFileEditor  
context menu id = #ClassFileEditorContext, #ClassFileRulerContext  
3. Compilation Unit Editor (\*.java)  
editor id = org.eclipse.jdt.ui.CompliationUnitEditor  
context menu id = #CompilationUnitEditorContext, #CompilationUnitRulerContext  
4. Default Text Editor  
editor id = org.eclipse.ui.DefaultTextEditor  
context menu id = #TextEditorContext, #TextRulerContext  
5. Snippet Editor (\*.jpage)  
editor id = org.eclipse.jdt.debug.ui.SnippetEditor  
context menu id = #JavaSnippetEditorContext, #JavaSnippetRulerContext  
